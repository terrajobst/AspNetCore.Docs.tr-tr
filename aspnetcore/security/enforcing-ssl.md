---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Bir ASP.NET Core web uygulamasını HTTPS/TLS gerektirir öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: security/enforcing-ssl
ms.openlocfilehash: a5359fe49e71ab59b47a8a5a39e7b806ad308235
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090997"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="457a8-103">ASP.NET core'da HTTPS'yi zorunlu kılma</span><span class="sxs-lookup"><span data-stu-id="457a8-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="457a8-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="457a8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="457a8-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="457a8-105">This document shows how to:</span></span>

* <span data-ttu-id="457a8-106">Tüm istekler için HTTPS gerektirir.</span><span class="sxs-lookup"><span data-stu-id="457a8-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="457a8-107">Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="457a8-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="457a8-108">Hiçbir API, bir istemci ilk istek üzerine hassas verileri göndermesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="457a8-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="457a8-109">Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="457a8-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="457a8-110">`RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="457a8-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="457a8-111">API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="457a8-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="457a8-112">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="457a8-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="457a8-113">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="457a8-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="457a8-114">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="457a8-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="457a8-115">400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.</span><span class="sxs-lookup"><span data-stu-id="457a8-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="457a8-116">HTTPS'yi zorunlu</span><span class="sxs-lookup"><span data-stu-id="457a8-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="457a8-117">Bu üretim ASP.NET Core web uygulamaları çağrısı öneririz:</span><span class="sxs-lookup"><span data-stu-id="457a8-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="457a8-118">HTTPS yeniden yönlendirmesi ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) HTTP isteklerini HTTPS için yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="457a8-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="457a8-119">HSTS ara yazılımı ([UseHsts](#http-strict-transport-security-protocol-hsts)) HTTP katı Aktarım güvenlik protokolü (HSTS) üst bilgileri istemcilere göndermek için.</span><span class="sxs-lookup"><span data-stu-id="457a8-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="457a8-120">Proxy bağlantı güvenliği (HTTPS) işlemek bir ters proxy yapılandırmasıyla dağıtılan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="457a8-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="457a8-121">Proxy de HTTPS yeniden yönlendirmesi işliyorsa, HTTPS yeniden yönlendirmesi ara yazılım kullanmaya gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="457a8-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="457a8-122">Proxy sunucusu, aynı zamanda HSTS üstbilgileri yazma işleyen varsa (örneğin, [yerel HSTS desteği (1709) IIS 10.0 veya üzeri](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS ara yazılım, uygulama tarafından gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="457a8-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="457a8-123">Daha fazla bilgi için [geri çevirme HTTPS/HSTS, proje oluşturma sırasında](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="457a8-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="457a8-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="457a8-124">UseHttpsRedirection</span></span>

<span data-ttu-id="457a8-125">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="457a8-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="457a8-126">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="457a8-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="457a8-127">Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="457a8-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="457a8-128">Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) tarafından geçersiz kılınmadığı sürece `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="457a8-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="457a8-129">Kalıcı yeniden yönlendirmeleri yerine geçici yeniden yönlendirmeleri kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="457a8-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="457a8-130">Bağlantıyı önbelleğe almayı, geliştirme ortamlarında tutarsız davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="457a8-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="457a8-131">Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermek isterseniz, bkz. [yeniden yönlendirmeleri kalıcı olarak üretim ortamında yapılandırma](#configure-permanent-redirects-in-production) bölümü.</span><span class="sxs-lookup"><span data-stu-id="457a8-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="457a8-132">Kullanmanızı öneririz [HSTS](#http-strict-transport-security-protocol-hsts) yalnızca kaynak güvenli istemcileri için sinyal istekleri uygulamada (yalnızca üretim) gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="457a8-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="457a8-133">Bağlantı noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="457a8-133">Port configuration</span></span>

<span data-ttu-id="457a8-134">Güvenli olmayan bir istek HTTPS'ye yönlendirmek için bir bağlantı noktası için ara yazılımı kullanılabilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="457a8-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="457a8-135">Bağlantı noktası varsa:</span><span class="sxs-lookup"><span data-stu-id="457a8-135">If no port is available:</span></span>

* <span data-ttu-id="457a8-136">HTTPS için yeniden yönlendirme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="457a8-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="457a8-137">Ara yazılım, "yeniden yönlendirme için https bağlantı noktasını belirlemek için başarısız oldu." uyarısı günlüğe kaydeder</span><span class="sxs-lookup"><span data-stu-id="457a8-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="457a8-138">Aşağıdaki yaklaşımlardan birini kullanarak HTTPS bağlantı noktasını belirtin:</span><span class="sxs-lookup"><span data-stu-id="457a8-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="457a8-139">Ayarlama [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="457a8-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="457a8-140">Ayarlama `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="457a8-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="457a8-141">**Anahtar**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="457a8-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="457a8-142">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="457a8-142">**Type**: *string*</span></span>  
  <span data-ttu-id="457a8-143">**Varsayılan**: varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="457a8-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="457a8-144">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="457a8-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="457a8-145">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (ön ek `ASPNETCORE_` kullanırken [Web ana bilgisayarı](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="457a8-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="457a8-146">Yapılandırma sırasında bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> içinde `Program`:</span><span class="sxs-lookup"><span data-stu-id="457a8-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="457a8-147">Güvenli düzenini kullanarak bir bağlantı noktası belirtmek `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="457a8-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="457a8-148">Ortam değişkenini sunucusunu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="457a8-148">The environment variable configures the server.</span></span> <span data-ttu-id="457a8-149">Ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="457a8-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="457a8-150">(Mu **değil** ters proxy dağıtımlarında çalışır.)</span><span class="sxs-lookup"><span data-stu-id="457a8-150">(Does **not** work in reverse proxy deployments.)</span></span>
* <span data-ttu-id="457a8-151">Bir HTTPS URL'si, geliştirme kümesinde *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="457a8-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="457a8-152">IIS Express kullanıldığında HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="457a8-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="457a8-153">Genel kullanıma yönelik edge dağıtımı için bir HTTPS URL'si uç nokta yapılandırma [Kestrel](xref:fundamentals/servers/kestrel) veya [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="457a8-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="457a8-154">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="457a8-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="457a8-155">Ara yazılım aracılığıyla bağlantı noktası bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="457a8-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="457a8-156">Bir uygulama bir ters proxy (örneğin, IIS, IIS Express) çalıştırın, `IServerAddressesFeature` kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="457a8-156">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="457a8-157">Bağlantı noktasını el ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="457a8-157">The port must be manually configured.</span></span> <span data-ttu-id="457a8-158">Bağlantı noktası olarak değil, istekleri yeniden yönlendirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="457a8-158">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="457a8-159">Kestrel'i veya HTTP.sys genel kullanıma yönelik bir uç sunucusu olarak kullanıldığında, hem de dinlenecek Kestrel veya HTTP.sys yapılandırılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="457a8-159">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="457a8-160">İstemci yeniden yönlendirilmiş burada güvenli bağlantı noktası (genellikle, üretim ve geliştirme 5001 443).</span><span class="sxs-lookup"><span data-stu-id="457a8-160">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="457a8-161">Güvensiz bağlantı noktası (genellikle, üretimde 80) ile 5000 geliştirme.</span><span class="sxs-lookup"><span data-stu-id="457a8-161">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="457a8-162">Güvensiz bağlantı güvenli olmayan bir istek almak ve güvenli bağlantı noktasına istemciyi yeniden yönlendirmek için uygulamanın sırası istemci tarafından erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="457a8-162">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="457a8-163">Daha fazla bilgi için [Kestrel uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) veya <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="457a8-163">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="457a8-164">Dağıtım senaryoları</span><span class="sxs-lookup"><span data-stu-id="457a8-164">Deployment scenarios</span></span>

<span data-ttu-id="457a8-165">İstemci ve sunucu arasında herhangi bir güvenlik duvarını Ayrıca iletişim bağlantı noktaları trafik için açık olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="457a8-165">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="457a8-166">İstekleri bir ters proxy yapılandırma iletilir kullanırsanız [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) çağırmadan önce HTTPS yeniden yönlendirmesi ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="457a8-166">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="457a8-167">Üst bilgileri ara yazılım güncelleştirmeleri iletilen `Request.Scheme`kullanarak `X-Forwarded-Proto` başlığı.</span><span class="sxs-lookup"><span data-stu-id="457a8-167">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="457a8-168">Ara yazılım verir yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="457a8-168">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="457a8-169">İletilen üstbilgileri ara yazılımı kullanılmaz, arka uç uygulama doğru düzenini almak değil ve bir yeniden yönlendirme döngüsüne.</span><span class="sxs-lookup"><span data-stu-id="457a8-169">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="457a8-170">Ortak bir son kullanıcı hata iletisi, çok fazla yeniden yönlendirmeleri oluşmuş ' dir.</span><span class="sxs-lookup"><span data-stu-id="457a8-170">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="457a8-171">Azure App Service'e dağıtım yaparken, sunulan yönergeleri [öğretici: Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="457a8-171">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="457a8-172">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="457a8-172">Options</span></span>

<span data-ttu-id="457a8-173">Aşağıdaki vurgulanmış kodu çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="457a8-173">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="457a8-174">Çağırma `AddHttpsRedirection` yalnızca değerlerini değiştirmek gerekli olan `HttpsPort` veya `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="457a8-174">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="457a8-175">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="457a8-175">The preceding highlighted code:</span></span>

* <span data-ttu-id="457a8-176">Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) için <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="457a8-176">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="457a8-177">Alanlarını kullanın <xref:Microsoft.AspNetCore.Http.StatusCodes> sınıfı atamaların `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="457a8-177">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="457a8-178">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="457a8-178">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="457a8-179">443 varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="457a8-179">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="457a8-180">Üretim ortamında yeniden yönlendirmeleri kalıcı olarak yapılandırma</span><span class="sxs-lookup"><span data-stu-id="457a8-180">Configure permanent redirects in production</span></span>

<span data-ttu-id="457a8-181">Ara yazılım varsayılan olarak göndermek için bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) ile tüm yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="457a8-181">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="457a8-182">Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermeyi tercih ederseniz, ara yazılım seçenekleri yapılandırma koşullu bir denetimi geliştirme ortam için kaydırın.</span><span class="sxs-lookup"><span data-stu-id="457a8-182">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="457a8-183">Yapılandırma sırasında bir `IWebHostBuilder` içinde *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="457a8-183">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="457a8-184">HTTPS yeniden yönlendirmesi ara yazılım alternatif bir yaklaşım</span><span class="sxs-lookup"><span data-stu-id="457a8-184">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="457a8-185">HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="457a8-185">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="457a8-186">`AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="457a8-186">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="457a8-187">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="457a8-187">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="457a8-188">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="457a8-188">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="457a8-189">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS'yi zorunlu tutmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="457a8-189">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="457a8-190">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri donatmak veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="457a8-190">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="457a8-191">Genel öznitelik uygulamak için aşağıdaki kodu ekleyin. `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="457a8-191">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="457a8-192">Tüm istekleri kullanmak önceki vurgulanan kod gerektirir `HTTPS`; bu nedenle, HTTP isteklerini dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="457a8-192">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="457a8-193">Aşağıdaki vurgulanmış kodu tüm HTTP isteklerini HTTPS için yeniden yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="457a8-193">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="457a8-194">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="457a8-194">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="457a8-195">Ara yazılım Ayrıca, uygulama yeniden yönlendirme yürütüldüğünde, durum kodu veya durum kodunu ve bağlantı noktası ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="457a8-195">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="457a8-196">Genel olarak HTTPS'yi zorunlu (`options.Filters.Add(new RequireHttpsAttribute());`) güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="457a8-196">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="457a8-197">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfaları için öznitelik değil olarak kabul genel HTTPS'yi zorunlu kadar güvenli.</span><span class="sxs-lookup"><span data-stu-id="457a8-197">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="457a8-198">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfaları eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="457a8-198">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="457a8-199">HTTP taşıma katı güvenlik protokolü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="457a8-199">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="457a8-200">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="457a8-200">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="457a8-201">Olduğunda bir [HSTS destekleyen bir tarayıcı](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) bu üstbilgiyi alır:</span><span class="sxs-lookup"><span data-stu-id="457a8-201">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="457a8-202">Tarayıcıya HTTP üzerinden herhangi bir iletişim göndermeyi önler etki alanı için yapılandırma depolar.</span><span class="sxs-lookup"><span data-stu-id="457a8-202">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="457a8-203">Tarayıcı tüm iletişim HTTPS üzerinden zorlar.</span><span class="sxs-lookup"><span data-stu-id="457a8-203">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="457a8-204">Tarayıcı kullanıcı, güvenilmeyen ya da geçersiz bir sertifika kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="457a8-204">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="457a8-205">Tarayıcı geçici olarak bu tür bir sertifika güven etmesine istemlerini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="457a8-205">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="457a8-206">İstemci tarafından HSTS zorunlu kılındığından bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="457a8-206">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="457a8-207">İstemci HSTS desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="457a8-207">The client must support HSTS.</span></span>
* <span data-ttu-id="457a8-208">HSTS HSTS ilkesi oluşturmak üzere en az bir başarılı HTTPS isteğini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="457a8-208">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="457a8-209">Uygulama gerekir her HTTP isteği denetleyin ve yeniden yönlendirme veya HTTP isteği reddedebilir.</span><span class="sxs-lookup"><span data-stu-id="457a8-209">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="457a8-210">ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="457a8-210">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="457a8-211">Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="457a8-211">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="457a8-212">`UseHsts` HSTS ayarları yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez.</span><span class="sxs-lookup"><span data-stu-id="457a8-212">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="457a8-213">Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.</span><span class="sxs-lookup"><span data-stu-id="457a8-213">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="457a8-214">Üretim ortamları için ilk ayarlamak için ilk kez, HTTPS uygulama [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) birini kullanarak küçük bir değere <xref:System.TimeSpan> yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="457a8-214">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="457a8-215">HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="457a8-215">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="457a8-216">HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="457a8-216">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="457a8-217">Aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="457a8-217">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="457a8-218">Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="457a8-218">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="457a8-219">Önyükleme bölümü olmayan [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak.</span><span class="sxs-lookup"><span data-stu-id="457a8-219">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="457a8-220">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="457a8-220">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="457a8-221">Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır.</span><span class="sxs-lookup"><span data-stu-id="457a8-221">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="457a8-222">Açıkça Strıct aktarım güvenliği üst bilgi, max-age parametresini 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="457a8-222">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="457a8-223">Aksi durumda, 30 gün için varsayılanları ayarlama.</span><span class="sxs-lookup"><span data-stu-id="457a8-223">If not set, defaults to 30 days.</span></span> <span data-ttu-id="457a8-224">Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="457a8-224">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="457a8-225">Ekler `example.com` dışlanacak konaklar listesine.</span><span class="sxs-lookup"><span data-stu-id="457a8-225">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="457a8-226">`UseHsts` şu geri döngü konakları hariç tutar:</span><span class="sxs-lookup"><span data-stu-id="457a8-226">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="457a8-227">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="457a8-227">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="457a8-228">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="457a8-228">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="457a8-229">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="457a8-229">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="457a8-230">Çevirme HTTPS/HSTS, proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="457a8-230">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="457a8-231">Bağlantı güvenliği ağ genel kullanıma yönelik ucuna nerede işlendiğini bazı arka uç hizmeti senaryolarda, her düğümde bağlantı güvenliği yapılandırma gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="457a8-231">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="457a8-232">Web uygulamaları Visual Studio'da veya gelen şablonlardan oluşturulan [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu etkinleştirme [HTTPS yeniden yönlendirmesi](#require-https) ve [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="457a8-232">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="457a8-233">Bu senaryolar gerektirmeyen dağıtımları için HTTPS/HSTS şablondan uygulama oluşturulduğunda çevirme.</span><span class="sxs-lookup"><span data-stu-id="457a8-233">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="457a8-234">Çevirme HTTPS/HSTS için:</span><span class="sxs-lookup"><span data-stu-id="457a8-234">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="457a8-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="457a8-235">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="457a8-236">Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="457a8-236">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![HTTPS onay kutusu seçili yapılandırma gösteren yeni ASP.NET Core Web uygulaması iletişim kutusu.](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="457a8-238">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="457a8-238">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="457a8-239">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="457a8-239">Use the `--no-https` option.</span></span> <span data-ttu-id="457a8-240">Örneğin</span><span class="sxs-lookup"><span data-stu-id="457a8-240">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="457a8-241">Docker için bir geliştirici sertifikası ayarlama</span><span class="sxs-lookup"><span data-stu-id="457a8-241">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="457a8-242">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="457a8-242">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="457a8-243">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="457a8-243">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="457a8-244">Apache ile Linux'ta ASP.NET Core barındıran: SSL yapılandırması</span><span class="sxs-lookup"><span data-stu-id="457a8-244">Host ASP.NET Core on Linux with Apache: SSL configuration</span></span>](xref:host-and-deploy/linux-apache#ssl-configuration)
* [<span data-ttu-id="457a8-245">Nginx ile Linux'ta ASP.NET Core barındıran: SSL yapılandırması</span><span class="sxs-lookup"><span data-stu-id="457a8-245">Host ASP.NET Core on Linux with Nginx: SSL configuration</span></span>](xref:host-and-deploy/linux-nginx#configure-ssl)
* [<span data-ttu-id="457a8-246">IIS'de SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="457a8-246">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="457a8-247">OWASP HSTS tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="457a8-247">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
