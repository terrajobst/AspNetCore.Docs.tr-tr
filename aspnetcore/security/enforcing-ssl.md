---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Bir ASP.NET Core web uygulamasını HTTPS/TLS gerektirir öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 8d48877153d6d75348e29299c669125904236de8
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692596"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="710db-103">ASP.NET core'da HTTPS'yi zorunlu kılma</span><span class="sxs-lookup"><span data-stu-id="710db-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="710db-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="710db-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="710db-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="710db-105">This document shows how to:</span></span>

* <span data-ttu-id="710db-106">Tüm istekler için HTTPS gerektirir.</span><span class="sxs-lookup"><span data-stu-id="710db-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="710db-107">Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="710db-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="710db-108">Hiçbir API, bir istemci ilk istek üzerine hassas verileri göndermesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="710db-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="710db-109">Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="710db-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="710db-110">`RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="710db-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="710db-111">API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="710db-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="710db-112">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="710db-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="710db-113">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="710db-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="710db-114">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="710db-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="710db-115">400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.</span><span class="sxs-lookup"><span data-stu-id="710db-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="710db-116">HTTPS'yi zorunlu</span><span class="sxs-lookup"><span data-stu-id="710db-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="710db-117">Bu üretim ASP.NET Core web uygulamaları çağrısı öneririz:</span><span class="sxs-lookup"><span data-stu-id="710db-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="710db-118">HTTPS yeniden yönlendirmesi ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) HTTP isteklerini HTTPS için yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="710db-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="710db-119">HSTS ara yazılımı ([UseHsts](#http-strict-transport-security-protocol-hsts)) HTTP katı Aktarım güvenlik protokolü (HSTS) üst bilgileri istemcilere göndermek için.</span><span class="sxs-lookup"><span data-stu-id="710db-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="710db-120">Proxy bağlantı güvenliği (HTTPS) işlemek bir ters proxy yapılandırmasıyla dağıtılan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="710db-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="710db-121">Proxy de HTTPS yeniden yönlendirmesi işliyorsa, HTTPS yeniden yönlendirmesi ara yazılım kullanmaya gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="710db-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="710db-122">Proxy sunucusu, aynı zamanda HSTS üstbilgileri yazma işleyen varsa (örneğin, [yerel HSTS desteği (1709) IIS 10.0 veya üzeri](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS ara yazılım, uygulama tarafından gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="710db-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="710db-123">Daha fazla bilgi için [geri çevirme HTTPS/HSTS, proje oluşturma sırasında](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="710db-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="710db-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="710db-124">UseHttpsRedirection</span></span>

<span data-ttu-id="710db-125">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="710db-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="710db-126">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="710db-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="710db-127">Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="710db-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="710db-128">Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) tarafından geçersiz kılınmadığı sürece `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="710db-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="710db-129">Kalıcı yeniden yönlendirmeleri yerine geçici yeniden yönlendirmeleri kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="710db-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="710db-130">Bağlantıyı önbelleğe almayı, geliştirme ortamlarında tutarsız davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="710db-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="710db-131">Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermek isterseniz, bkz. [yeniden yönlendirmeleri kalıcı olarak üretim ortamında yapılandırma](#configure-permanent-redirects-in-production) bölümü.</span><span class="sxs-lookup"><span data-stu-id="710db-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="710db-132">Kullanmanızı öneririz [HSTS](#http-strict-transport-security-protocol-hsts) yalnızca kaynak güvenli istemcileri için sinyal istekleri uygulamada (yalnızca üretim) gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="710db-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="710db-133">Bağlantı noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="710db-133">Port configuration</span></span>

<span data-ttu-id="710db-134">Güvenli olmayan bir istek HTTPS'ye yönlendirmek için bir bağlantı noktası için ara yazılımı kullanılabilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="710db-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="710db-135">Bağlantı noktası varsa:</span><span class="sxs-lookup"><span data-stu-id="710db-135">If no port is available:</span></span>

* <span data-ttu-id="710db-136">HTTPS için yeniden yönlendirme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="710db-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="710db-137">Ara yazılım, "yeniden yönlendirme için https bağlantı noktasını belirlemek için başarısız oldu." uyarısı günlüğe kaydeder</span><span class="sxs-lookup"><span data-stu-id="710db-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="710db-138">Aşağıdaki yaklaşımlardan birini kullanarak HTTPS bağlantı noktasını belirtin:</span><span class="sxs-lookup"><span data-stu-id="710db-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="710db-139">Ayarlama [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="710db-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="710db-140">Ayarlama `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="710db-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="710db-141">**Anahtar**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="710db-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="710db-142">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="710db-142">**Type**: *string*</span></span>  
  <span data-ttu-id="710db-143">**Varsayılan**: Varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="710db-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="710db-144">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="710db-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="710db-145">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (Önek `ASPNETCORE_` kullanırken [Web ana bilgisayarı](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="710db-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="710db-146">Yapılandırma sırasında bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> içinde `Program`:</span><span class="sxs-lookup"><span data-stu-id="710db-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="710db-147">Güvenli düzenini kullanarak bir bağlantı noktası belirtmek `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="710db-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="710db-148">Ortam değişkenini sunucusunu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="710db-148">The environment variable configures the server.</span></span> <span data-ttu-id="710db-149">Ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="710db-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="710db-150">Bu yaklaşım ters proxy dağıtımlarda çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="710db-150">This approach doesn't work in reverse proxy deployments.</span></span>
* <span data-ttu-id="710db-151">Bir HTTPS URL'si, geliştirme kümesinde *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="710db-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="710db-152">IIS Express kullanıldığında HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="710db-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="710db-153">Genel kullanıma yönelik edge dağıtımı için bir HTTPS URL'si uç nokta yapılandırma [Kestrel](xref:fundamentals/servers/kestrel) sunucu veya [HTTP.sys](xref:fundamentals/servers/httpsys) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="710db-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="710db-154">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="710db-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="710db-155">Ara yazılım aracılığıyla bağlantı noktası bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="710db-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="710db-156">Bir ters proxy yapılandırması'nda bir uygulama çalıştırıldığında <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="710db-156">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="710db-157">Bu bölümde açıklanan yaklaşımlardan birini kullanarak bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="710db-157">Set the port using one of the other approaches described in this section.</span></span>

<span data-ttu-id="710db-158">Kestrel'i veya HTTP.sys genel kullanıma yönelik bir uç sunucusu olarak kullanıldığında, hem de dinlenecek Kestrel veya HTTP.sys yapılandırılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="710db-158">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="710db-159">İstemci yeniden yönlendirilmiş burada güvenli bağlantı noktası (genellikle, üretim ve geliştirme 5001 443).</span><span class="sxs-lookup"><span data-stu-id="710db-159">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="710db-160">Güvensiz bağlantı noktası (genellikle, üretimde 80) ile 5000 geliştirme.</span><span class="sxs-lookup"><span data-stu-id="710db-160">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="710db-161">Güvensiz bağlantı güvenli olmayan bir istek almak ve güvenli bağlantı noktasına istemciyi yeniden yönlendirmek için uygulamanın sırası istemci tarafından erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="710db-161">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="710db-162">Daha fazla bilgi için [Kestrel uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) veya <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="710db-162">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="710db-163">Dağıtım senaryoları</span><span class="sxs-lookup"><span data-stu-id="710db-163">Deployment scenarios</span></span>

<span data-ttu-id="710db-164">İstemci ve sunucu arasında herhangi bir güvenlik duvarını Ayrıca iletişim bağlantı noktaları trafik için açık olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="710db-164">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="710db-165">İstekleri bir ters proxy yapılandırma iletilir kullanırsanız [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) çağırmadan önce HTTPS yeniden yönlendirmesi ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="710db-165">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="710db-166">Üst bilgileri ara yazılım güncelleştirmeleri iletilen `Request.Scheme`kullanarak `X-Forwarded-Proto` başlığı.</span><span class="sxs-lookup"><span data-stu-id="710db-166">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="710db-167">Ara yazılım verir yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="710db-167">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="710db-168">İletilen üstbilgileri ara yazılımı kullanılmaz, arka uç uygulama doğru düzenini almak değil ve bir yeniden yönlendirme döngüsüne.</span><span class="sxs-lookup"><span data-stu-id="710db-168">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="710db-169">Ortak bir son kullanıcı hata iletisi, çok fazla yeniden yönlendirmeleri oluşmuş ' dir.</span><span class="sxs-lookup"><span data-stu-id="710db-169">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="710db-170">Azure App Service'e dağıtım yaparken, sunulan yönergeleri [Öğreticisi: Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="710db-170">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="710db-171">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="710db-171">Options</span></span>

<span data-ttu-id="710db-172">Aşağıdaki vurgulanmış kodu çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="710db-172">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="710db-173">Çağırma `AddHttpsRedirection` yalnızca değerlerini değiştirmek gerekli olan `HttpsPort` veya `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="710db-173">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="710db-174">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="710db-174">The preceding highlighted code:</span></span>

* <span data-ttu-id="710db-175">Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) için <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="710db-175">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="710db-176">Alanlarını kullanın <xref:Microsoft.AspNetCore.Http.StatusCodes> sınıfı atamaların `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="710db-176">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="710db-177">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="710db-177">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="710db-178">443 varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="710db-178">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="710db-179">Üretim ortamında yeniden yönlendirmeleri kalıcı olarak yapılandırma</span><span class="sxs-lookup"><span data-stu-id="710db-179">Configure permanent redirects in production</span></span>

<span data-ttu-id="710db-180">Ara yazılım varsayılan olarak göndermek için bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) ile tüm yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="710db-180">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="710db-181">Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermeyi tercih ederseniz, ara yazılım seçenekleri yapılandırma koşullu bir denetimi geliştirme ortam için kaydırın.</span><span class="sxs-lookup"><span data-stu-id="710db-181">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="710db-182">Yapılandırma sırasında bir `IWebHostBuilder` içinde *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="710db-182">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

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

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="710db-183">HTTPS yeniden yönlendirmesi ara yazılım alternatif bir yaklaşım</span><span class="sxs-lookup"><span data-stu-id="710db-183">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="710db-184">HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="710db-184">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="710db-185">`AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="710db-185">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="710db-186">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="710db-186">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="710db-187">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="710db-187">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="710db-188">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS'yi zorunlu tutmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="710db-188">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="710db-189">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri donatmak veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="710db-189">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="710db-190">Genel öznitelik uygulamak için aşağıdaki kodu ekleyin. `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="710db-190">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="710db-191">Tüm istekleri kullanmak önceki vurgulanan kod gerektirir `HTTPS`; bu nedenle, HTTP isteklerini dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="710db-191">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="710db-192">Aşağıdaki vurgulanmış kodu tüm HTTP isteklerini HTTPS için yeniden yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="710db-192">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="710db-193">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="710db-193">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="710db-194">Ara yazılım Ayrıca, uygulama yeniden yönlendirme yürütüldüğünde, durum kodu veya durum kodunu ve bağlantı noktası ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="710db-194">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="710db-195">Genel olarak HTTPS'yi zorunlu (`options.Filters.Add(new RequireHttpsAttribute());`) güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="710db-195">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="710db-196">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfaları için öznitelik değil olarak kabul genel HTTPS'yi zorunlu kadar güvenli.</span><span class="sxs-lookup"><span data-stu-id="710db-196">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="710db-197">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfaları eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="710db-197">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="710db-198">HTTP taşıma katı güvenlik protokolü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="710db-198">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="710db-199">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="710db-199">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="710db-200">Olduğunda bir [HSTS destekleyen bir tarayıcı](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) bu üstbilgiyi alır:</span><span class="sxs-lookup"><span data-stu-id="710db-200">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="710db-201">Tarayıcıya HTTP üzerinden herhangi bir iletişim göndermeyi önler etki alanı için yapılandırma depolar.</span><span class="sxs-lookup"><span data-stu-id="710db-201">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="710db-202">Tarayıcı tüm iletişim HTTPS üzerinden zorlar.</span><span class="sxs-lookup"><span data-stu-id="710db-202">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="710db-203">Tarayıcı kullanıcı, güvenilmeyen ya da geçersiz bir sertifika kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="710db-203">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="710db-204">Tarayıcı geçici olarak bu tür bir sertifika güven etmesine istemlerini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="710db-204">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="710db-205">İstemci tarafından HSTS zorunlu kılındığından bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="710db-205">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="710db-206">İstemci HSTS desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="710db-206">The client must support HSTS.</span></span>
* <span data-ttu-id="710db-207">HSTS HSTS ilkesi oluşturmak üzere en az bir başarılı HTTPS isteğini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="710db-207">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="710db-208">Uygulama gerekir her HTTP isteği denetleyin ve yeniden yönlendirme veya HTTP isteği reddedebilir.</span><span class="sxs-lookup"><span data-stu-id="710db-208">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="710db-209">ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="710db-209">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="710db-210">Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="710db-210">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="710db-211">`UseHsts` HSTS ayarları yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez.</span><span class="sxs-lookup"><span data-stu-id="710db-211">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="710db-212">Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.</span><span class="sxs-lookup"><span data-stu-id="710db-212">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="710db-213">Üretim ortamları için ilk ayarlamak için ilk kez, HTTPS uygulama [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) birini kullanarak küçük bir değere <xref:System.TimeSpan> yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="710db-213">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="710db-214">HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="710db-214">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="710db-215">HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="710db-215">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="710db-216">Aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="710db-216">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="710db-217">Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="710db-217">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="710db-218">Önyükleme bölümü olmayan [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak.</span><span class="sxs-lookup"><span data-stu-id="710db-218">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="710db-219">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="710db-219">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="710db-220">Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır.</span><span class="sxs-lookup"><span data-stu-id="710db-220">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="710db-221">Açıkça Strıct aktarım güvenliği üst bilgi, max-age parametresini 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="710db-221">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="710db-222">Aksi durumda, 30 gün için varsayılanları ayarlama.</span><span class="sxs-lookup"><span data-stu-id="710db-222">If not set, defaults to 30 days.</span></span> <span data-ttu-id="710db-223">Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="710db-223">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="710db-224">Ekler `example.com` dışlanacak konaklar listesine.</span><span class="sxs-lookup"><span data-stu-id="710db-224">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="710db-225">`UseHsts` şu geri döngü konakları hariç tutar:</span><span class="sxs-lookup"><span data-stu-id="710db-225">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="710db-226">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="710db-226">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="710db-227">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="710db-227">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="710db-228">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="710db-228">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="710db-229">Çevirme HTTPS/HSTS, proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="710db-229">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="710db-230">Bağlantı güvenliği ağ genel kullanıma yönelik ucuna nerede işlendiğini bazı arka uç hizmeti senaryolarda, her düğümde bağlantı güvenliği yapılandırma gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="710db-230">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="710db-231">Web uygulamaları Visual Studio'da veya gelen şablonlardan oluşturulan [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu etkinleştirme [HTTPS yeniden yönlendirmesi](#require-https) ve [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="710db-231">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="710db-232">Bu senaryolar gerektirmeyen dağıtımları için HTTPS/HSTS şablondan uygulama oluşturulduğunda çevirme.</span><span class="sxs-lookup"><span data-stu-id="710db-232">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="710db-233">Çevirme HTTPS/HSTS için:</span><span class="sxs-lookup"><span data-stu-id="710db-233">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="710db-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="710db-234">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="710db-235">Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="710db-235">Uncheck the **Configure for HTTPS** check box.</span></span>

![HTTPS onay kutusu seçili yapılandırma gösteren yeni ASP.NET Core Web uygulaması iletişim kutusu.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="710db-237">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="710db-237">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="710db-238">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="710db-238">Use the `--no-https` option.</span></span> <span data-ttu-id="710db-239">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="710db-239">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="710db-240">Windows ve macOS üzerinde ASP.NET Core HTTPS geliştirme sertifikasına güvenmek</span><span class="sxs-lookup"><span data-stu-id="710db-240">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="710db-241">.NET core SDK'sı, HTTPS geliştirme sertifikası içerir.</span><span class="sxs-lookup"><span data-stu-id="710db-241">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="710db-242">Sertifika, ilk kez çalıştırma deneyimi bir parçası olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="710db-242">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="710db-243">Örneğin, `dotnet --info` aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="710db-243">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="710db-244">.NET Core SDK'sını yükleme ASP.NET Core HTTPS geliştirme sertifikası yerel kullanıcı sertifika deposuna yükler.</span><span class="sxs-lookup"><span data-stu-id="710db-244">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="710db-245">Sertifika yüklü, ancak güvenilmeyen.</span><span class="sxs-lookup"><span data-stu-id="710db-245">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="710db-246">Sertifika güven için dotnet çalıştırmak için tek seferlik bir adım gerçekleştirmeniz `dev-certs` aracı:</span><span class="sxs-lookup"><span data-stu-id="710db-246">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="710db-247">Aşağıdaki komut hakkında Yardım sağlar. `dev-certs` aracı:</span><span class="sxs-lookup"><span data-stu-id="710db-247">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="710db-248">Docker için bir geliştirici sertifikası ayarlama</span><span class="sxs-lookup"><span data-stu-id="710db-248">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="710db-249">Bkz: [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="710db-249">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="710db-250">Linux için Windows alt sistemi HTTPS sertifika güven</span><span class="sxs-lookup"><span data-stu-id="710db-250">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="710db-251">Linux (WSL) için Windows alt sistemi HTTPS otomatik olarak imzalanan bir sertifika oluşturur. WSL sertifikasına güvenmek için Windows sertifika deposunu yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="710db-251">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="710db-252">Oluşturulan WSL sertifikasını dışarı aktarmak için aşağıdaki komutu çalıştırın: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="710db-252">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="710db-253">Bir WSL penceresinde aşağıdaki komutu çalıştırın: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="710db-253">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="710db-254">Linux, Windows güvenilir sertifika kullanması için yukarıdaki komut ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="710db-254">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="additional-information"></a><span data-ttu-id="710db-255">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="710db-255">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="710db-256">Apache ile Linux'ta ASP.NET Core barındırın: HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="710db-256">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="710db-257">Nginx ile Linux'ta ASP.NET Core barındırın: HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="710db-257">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="710db-258">IIS'de SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="710db-258">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="710db-259">OWASP HSTS tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="710db-259">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
