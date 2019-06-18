---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Bir ASP.NET Core web uygulamasını HTTPS/TLS gerektirir öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 08ce50775d1b5348cb0528a1724cec2e5c72dae2
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152906"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="4442b-103">ASP.NET core'da HTTPS'yi zorunlu kılma</span><span class="sxs-lookup"><span data-stu-id="4442b-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="4442b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4442b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4442b-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="4442b-105">This document shows how to:</span></span>

* <span data-ttu-id="4442b-106">Tüm istekler için HTTPS gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4442b-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="4442b-107">Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="4442b-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="4442b-108">Hiçbir API, bir istemci ilk istek üzerine hassas verileri göndermesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4442b-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="4442b-109">API projeleri</span><span class="sxs-lookup"><span data-stu-id="4442b-109">API projects</span></span>
>
> <span data-ttu-id="4442b-110">Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="4442b-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="4442b-111">`RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="4442b-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="4442b-112">API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="4442b-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="4442b-113">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="4442b-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="4442b-114">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="4442b-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="4442b-115">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="4442b-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="4442b-116">400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.</span><span class="sxs-lookup"><span data-stu-id="4442b-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="4442b-117">API projeleri</span><span class="sxs-lookup"><span data-stu-id="4442b-117">API projects</span></span>
>
> <span data-ttu-id="4442b-118">Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="4442b-118">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="4442b-119">`RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="4442b-119">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="4442b-120">API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="4442b-120">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="4442b-121">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="4442b-121">Such clients may send information over HTTP.</span></span> <span data-ttu-id="4442b-122">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="4442b-122">Web APIs should either:</span></span>
>
> * <span data-ttu-id="4442b-123">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="4442b-123">Not listen on HTTP.</span></span>
> * <span data-ttu-id="4442b-124">400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.</span><span class="sxs-lookup"><span data-stu-id="4442b-124">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="4442b-125">HSTS ve API projeleriniz</span><span class="sxs-lookup"><span data-stu-id="4442b-125">HSTS and API projects</span></span>
>
> <span data-ttu-id="4442b-126">Varsayılan API projeleri içermez [HSTS](#hsts) HSTS genellikle bir tarayıcı yalnızca yönergesi olduğu için.</span><span class="sxs-lookup"><span data-stu-id="4442b-126">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="4442b-127">Telefon veya Masaüstü uygulamaları gibi diğer çağıranlar yapmak **değil** yönerge uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="4442b-127">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="4442b-128">Bile tarayıcılar içinde tek bir kimlik doğrulaması çağrısı HTTP üzerinden bir API'ye güvenli olmayan ağlarda riskleri taşır.</span><span class="sxs-lookup"><span data-stu-id="4442b-128">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="4442b-129">Güvenli bir yöntem yalnızca dinlemek ve HTTPS üzerinden yanıt vermek için API projeleri yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="4442b-129">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="4442b-130">HTTPS'yi zorunlu</span><span class="sxs-lookup"><span data-stu-id="4442b-130">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4442b-131">Bu üretim ASP.NET Core web uygulamaları çağrısı öneririz:</span><span class="sxs-lookup"><span data-stu-id="4442b-131">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="4442b-132">HTTPS yeniden yönlendirmesi ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) HTTP isteklerini HTTPS için yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="4442b-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="4442b-133">HSTS ara yazılımı ([UseHsts](#http-strict-transport-security-protocol-hsts)) HTTP katı Aktarım güvenlik protokolü (HSTS) üst bilgileri istemcilere göndermek için.</span><span class="sxs-lookup"><span data-stu-id="4442b-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="4442b-134">Proxy bağlantı güvenliği (HTTPS) işlemek bir ters proxy yapılandırmasıyla dağıtılan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="4442b-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="4442b-135">Proxy de HTTPS yeniden yönlendirmesi işliyorsa, HTTPS yeniden yönlendirmesi ara yazılım kullanmaya gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="4442b-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="4442b-136">Proxy sunucusu, aynı zamanda HSTS üstbilgileri yazma işleyen varsa (örneğin, [yerel HSTS desteği (1709) IIS 10.0 veya üzeri](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS ara yazılım, uygulama tarafından gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4442b-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="4442b-137">Daha fazla bilgi için [geri çevirme HTTPS/HSTS, proje oluşturma sırasında](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="4442b-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="4442b-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="4442b-138">UseHttpsRedirection</span></span>

<span data-ttu-id="4442b-139">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="4442b-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="4442b-140">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="4442b-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="4442b-141">Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="4442b-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="4442b-142">Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) tarafından geçersiz kılınmadığı sürece `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="4442b-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="4442b-143">Kalıcı yeniden yönlendirmeleri yerine geçici yeniden yönlendirmeleri kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="4442b-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="4442b-144">Bağlantıyı önbelleğe almayı, geliştirme ortamlarında tutarsız davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="4442b-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="4442b-145">Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermek isterseniz, bkz. [yeniden yönlendirmeleri kalıcı olarak üretim ortamında yapılandırma](#configure-permanent-redirects-in-production) bölümü.</span><span class="sxs-lookup"><span data-stu-id="4442b-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="4442b-146">Kullanmanızı öneririz [HSTS](#http-strict-transport-security-protocol-hsts) yalnızca kaynak güvenli istemcileri için sinyal istekleri uygulamada (yalnızca üretim) gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4442b-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="4442b-147">Bağlantı noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4442b-147">Port configuration</span></span>

<span data-ttu-id="4442b-148">Güvenli olmayan bir istek HTTPS'ye yönlendirmek için bir bağlantı noktası için ara yazılımı kullanılabilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4442b-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="4442b-149">Bağlantı noktası varsa:</span><span class="sxs-lookup"><span data-stu-id="4442b-149">If no port is available:</span></span>

* <span data-ttu-id="4442b-150">HTTPS için yeniden yönlendirme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="4442b-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="4442b-151">Ara yazılım, "yeniden yönlendirme için https bağlantı noktasını belirlemek için başarısız oldu." uyarısı günlüğe kaydeder</span><span class="sxs-lookup"><span data-stu-id="4442b-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="4442b-152">Aşağıdaki yaklaşımlardan birini kullanarak HTTPS bağlantı noktasını belirtin:</span><span class="sxs-lookup"><span data-stu-id="4442b-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="4442b-153">Ayarlama [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="4442b-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="4442b-154">Ayarlama `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="4442b-154">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="4442b-155">**Anahtar**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="4442b-155">**Key**: `https_port`</span></span>  
  <span data-ttu-id="4442b-156">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="4442b-156">**Type**: *string*</span></span>  
  <span data-ttu-id="4442b-157">**Varsayılan**: Varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="4442b-157">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="4442b-158">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4442b-158">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="4442b-159">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (Önek `ASPNETCORE_` kullanırken [Web ana bilgisayarı](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="4442b-159">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="4442b-160">Yapılandırma sırasında bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> içinde `Program`:</span><span class="sxs-lookup"><span data-stu-id="4442b-160">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="4442b-161">Güvenli düzenini kullanarak bir bağlantı noktası belirtmek `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4442b-161">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="4442b-162">Ortam değişkenini sunucusunu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4442b-162">The environment variable configures the server.</span></span> <span data-ttu-id="4442b-163">Ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="4442b-163">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="4442b-164">Bu yaklaşım ters proxy dağıtımlarda çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="4442b-164">This approach doesn't work in reverse proxy deployments.</span></span>
* <span data-ttu-id="4442b-165">Bir HTTPS URL'si, geliştirme kümesinde *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4442b-165">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="4442b-166">IIS Express kullanıldığında HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4442b-166">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="4442b-167">Genel kullanıma yönelik edge dağıtımı için bir HTTPS URL'si uç nokta yapılandırma [Kestrel](xref:fundamentals/servers/kestrel) sunucu veya [HTTP.sys](xref:fundamentals/servers/httpsys) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="4442b-167">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="4442b-168">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4442b-168">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="4442b-169">Ara yazılım aracılığıyla bağlantı noktası bulur <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="4442b-169">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="4442b-170">Bir ters proxy yapılandırması'nda bir uygulama çalıştırıldığında <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4442b-170">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="4442b-171">Bu bölümde açıklanan yaklaşımlardan birini kullanarak bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4442b-171">Set the port using one of the other approaches described in this section.</span></span>

<span data-ttu-id="4442b-172">Kestrel'i veya HTTP.sys genel kullanıma yönelik bir uç sunucusu olarak kullanıldığında, hem de dinlenecek Kestrel veya HTTP.sys yapılandırılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="4442b-172">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="4442b-173">İstemci yeniden yönlendirilmiş burada güvenli bağlantı noktası (genellikle, üretim ve geliştirme 5001 443).</span><span class="sxs-lookup"><span data-stu-id="4442b-173">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="4442b-174">Güvensiz bağlantı noktası (genellikle, üretimde 80) ile 5000 geliştirme.</span><span class="sxs-lookup"><span data-stu-id="4442b-174">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="4442b-175">Güvensiz bağlantı güvenli olmayan bir istek almak ve güvenli bağlantı noktasına istemciyi yeniden yönlendirmek için uygulamanın sırası istemci tarafından erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4442b-175">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="4442b-176">Daha fazla bilgi için [Kestrel uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) veya <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="4442b-176">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="4442b-177">Dağıtım senaryoları</span><span class="sxs-lookup"><span data-stu-id="4442b-177">Deployment scenarios</span></span>

<span data-ttu-id="4442b-178">İstemci ve sunucu arasında herhangi bir güvenlik duvarını Ayrıca iletişim bağlantı noktaları trafik için açık olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4442b-178">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="4442b-179">İstekleri bir ters proxy yapılandırma iletilir kullanırsanız [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) çağırmadan önce HTTPS yeniden yönlendirmesi ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="4442b-179">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="4442b-180">Üst bilgileri ara yazılım güncelleştirmeleri iletilen `Request.Scheme`kullanarak `X-Forwarded-Proto` başlığı.</span><span class="sxs-lookup"><span data-stu-id="4442b-180">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="4442b-181">Ara yazılım verir yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="4442b-181">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="4442b-182">İletilen üstbilgileri ara yazılımı kullanılmaz, arka uç uygulama doğru düzenini almak değil ve bir yeniden yönlendirme döngüsüne.</span><span class="sxs-lookup"><span data-stu-id="4442b-182">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="4442b-183">Ortak bir son kullanıcı hata iletisi, çok fazla yeniden yönlendirmeleri oluşmuş ' dir.</span><span class="sxs-lookup"><span data-stu-id="4442b-183">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="4442b-184">Azure App Service'e dağıtım yaparken, sunulan yönergeleri [Öğreticisi: Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="4442b-184">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="4442b-185">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="4442b-185">Options</span></span>

<span data-ttu-id="4442b-186">Aşağıdaki vurgulanmış kodu çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="4442b-186">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="4442b-187">Çağırma `AddHttpsRedirection` yalnızca değerlerini değiştirmek gerekli olan `HttpsPort` veya `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="4442b-187">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="4442b-188">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="4442b-188">The preceding highlighted code:</span></span>

* <span data-ttu-id="4442b-189">Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) için <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="4442b-189">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="4442b-190">Alanlarını kullanın <xref:Microsoft.AspNetCore.Http.StatusCodes> sınıfı atamaların `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="4442b-190">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="4442b-191">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4442b-191">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="4442b-192">443 varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="4442b-192">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="4442b-193">Üretim ortamında yeniden yönlendirmeleri kalıcı olarak yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4442b-193">Configure permanent redirects in production</span></span>

<span data-ttu-id="4442b-194">Ara yazılım varsayılan olarak göndermek için bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) ile tüm yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="4442b-194">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="4442b-195">Uygulama geliştirme dışı bir ortamda olduğunda, kalıcı bir yeniden yönlendirme durum kodunu göndermeyi tercih ederseniz, ara yazılım seçenekleri yapılandırma koşullu bir denetimi geliştirme ortam için kaydırın.</span><span class="sxs-lookup"><span data-stu-id="4442b-195">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="4442b-196">Yapılandırma sırasında bir `IWebHostBuilder` içinde *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4442b-196">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

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

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="4442b-197">HTTPS yeniden yönlendirmesi ara yazılım alternatif bir yaklaşım</span><span class="sxs-lookup"><span data-stu-id="4442b-197">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="4442b-198">HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="4442b-198">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="4442b-199">`AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4442b-199">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="4442b-200">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="4442b-200">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="4442b-201">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="4442b-201">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="4442b-202">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS'yi zorunlu tutmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4442b-202">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="4442b-203">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri donatmak veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4442b-203">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="4442b-204">Genel öznitelik uygulamak için aşağıdaki kodu ekleyin. `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4442b-204">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="4442b-205">Tüm istekleri kullanmak önceki vurgulanan kod gerektirir `HTTPS`; bu nedenle, HTTP isteklerini dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="4442b-205">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="4442b-206">Aşağıdaki vurgulanmış kodu tüm HTTP isteklerini HTTPS için yeniden yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="4442b-206">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="4442b-207">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="4442b-207">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="4442b-208">Ara yazılım Ayrıca, uygulama yeniden yönlendirme yürütüldüğünde, durum kodu veya durum kodunu ve bağlantı noktası ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="4442b-208">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="4442b-209">Genel olarak HTTPS'yi zorunlu (`options.Filters.Add(new RequireHttpsAttribute());`) güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="4442b-209">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="4442b-210">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfaları için öznitelik değil olarak kabul genel HTTPS'yi zorunlu kadar güvenli.</span><span class="sxs-lookup"><span data-stu-id="4442b-210">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="4442b-211">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfaları eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4442b-211">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="4442b-212">HTTP taşıma katı güvenlik protokolü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="4442b-212">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="4442b-213">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="4442b-213">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="4442b-214">Olduğunda bir [HSTS destekleyen bir tarayıcı](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) bu üstbilgiyi alır:</span><span class="sxs-lookup"><span data-stu-id="4442b-214">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="4442b-215">Tarayıcıya HTTP üzerinden herhangi bir iletişim göndermeyi önler etki alanı için yapılandırma depolar.</span><span class="sxs-lookup"><span data-stu-id="4442b-215">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="4442b-216">Tarayıcı tüm iletişim HTTPS üzerinden zorlar.</span><span class="sxs-lookup"><span data-stu-id="4442b-216">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="4442b-217">Tarayıcı kullanıcı, güvenilmeyen ya da geçersiz bir sertifika kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="4442b-217">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="4442b-218">Tarayıcı geçici olarak bu tür bir sertifika güven etmesine istemlerini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="4442b-218">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="4442b-219">İstemci tarafından HSTS zorunlu kılındığından bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="4442b-219">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="4442b-220">İstemci HSTS desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4442b-220">The client must support HSTS.</span></span>
* <span data-ttu-id="4442b-221">HSTS HSTS ilkesi oluşturmak üzere en az bir başarılı HTTPS isteğini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4442b-221">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="4442b-222">Uygulama gerekir her HTTP isteği denetleyin ve yeniden yönlendirme veya HTTP isteği reddedebilir.</span><span class="sxs-lookup"><span data-stu-id="4442b-222">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="4442b-223">ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4442b-223">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="4442b-224">Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="4442b-224">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="4442b-225">`UseHsts` HSTS ayarları yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez.</span><span class="sxs-lookup"><span data-stu-id="4442b-225">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="4442b-226">Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.</span><span class="sxs-lookup"><span data-stu-id="4442b-226">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="4442b-227">Üretim ortamları için ilk ayarlamak için ilk kez, HTTPS uygulama [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) birini kullanarak küçük bir değere <xref:System.TimeSpan> yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="4442b-227">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="4442b-228">HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4442b-228">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="4442b-229">HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="4442b-229">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="4442b-230">Aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="4442b-230">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="4442b-231">Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4442b-231">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="4442b-232">Önyükleme bölümü olmayan [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak.</span><span class="sxs-lookup"><span data-stu-id="4442b-232">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="4442b-233">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4442b-233">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="4442b-234">Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4442b-234">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="4442b-235">Açıkça Strıct aktarım güvenliği üst bilgi, max-age parametresini 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4442b-235">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="4442b-236">Aksi durumda, 30 gün için varsayılanları ayarlama.</span><span class="sxs-lookup"><span data-stu-id="4442b-236">If not set, defaults to 30 days.</span></span> <span data-ttu-id="4442b-237">Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4442b-237">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="4442b-238">Ekler `example.com` dışlanacak konaklar listesine.</span><span class="sxs-lookup"><span data-stu-id="4442b-238">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="4442b-239">`UseHsts` şu geri döngü konakları hariç tutar:</span><span class="sxs-lookup"><span data-stu-id="4442b-239">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="4442b-240">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="4442b-240">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="4442b-241">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="4442b-241">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="4442b-242">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="4442b-242">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="4442b-243">Çevirme HTTPS/HSTS, proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="4442b-243">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="4442b-244">Bağlantı güvenliği ağ genel kullanıma yönelik ucuna nerede işlendiğini bazı arka uç hizmeti senaryolarda, her düğümde bağlantı güvenliği yapılandırma gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4442b-244">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="4442b-245">Web uygulamaları Visual Studio'da veya gelen şablonlardan oluşturulan [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu etkinleştirme [HTTPS yeniden yönlendirmesi](#require-https) ve [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="4442b-245">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="4442b-246">Bu senaryolar gerektirmeyen dağıtımları için HTTPS/HSTS şablondan uygulama oluşturulduğunda çevirme.</span><span class="sxs-lookup"><span data-stu-id="4442b-246">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="4442b-247">Çevirme HTTPS/HSTS için:</span><span class="sxs-lookup"><span data-stu-id="4442b-247">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4442b-248">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4442b-248">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="4442b-249">Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="4442b-249">Uncheck the **Configure for HTTPS** check box.</span></span>

![HTTPS onay kutusu seçili yapılandırma gösteren yeni ASP.NET Core Web uygulaması iletişim kutusu.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4442b-251">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4442b-251">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="4442b-252">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4442b-252">Use the `--no-https` option.</span></span> <span data-ttu-id="4442b-253">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4442b-253">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="4442b-254">Windows ve macOS üzerinde ASP.NET Core HTTPS geliştirme sertifikasına güvenmek</span><span class="sxs-lookup"><span data-stu-id="4442b-254">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="4442b-255">.NET core SDK'sı, HTTPS geliştirme sertifikası içerir.</span><span class="sxs-lookup"><span data-stu-id="4442b-255">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="4442b-256">Sertifika, ilk kez çalıştırma deneyimi bir parçası olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4442b-256">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="4442b-257">Örneğin, `dotnet --info` aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="4442b-257">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="4442b-258">.NET Core SDK'sını yükleme ASP.NET Core HTTPS geliştirme sertifikası yerel kullanıcı sertifika deposuna yükler.</span><span class="sxs-lookup"><span data-stu-id="4442b-258">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="4442b-259">Sertifika yüklü, ancak güvenilmeyen.</span><span class="sxs-lookup"><span data-stu-id="4442b-259">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="4442b-260">Sertifika güven için dotnet çalıştırmak için tek seferlik bir adım gerçekleştirmeniz `dev-certs` aracı:</span><span class="sxs-lookup"><span data-stu-id="4442b-260">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="4442b-261">Aşağıdaki komut hakkında Yardım sağlar. `dev-certs` aracı:</span><span class="sxs-lookup"><span data-stu-id="4442b-261">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="4442b-262">Docker için bir geliştirici sertifikası ayarlama</span><span class="sxs-lookup"><span data-stu-id="4442b-262">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="4442b-263">Bkz: [bu GitHub sorunu](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="4442b-263">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="4442b-264">Linux için Windows alt sistemi HTTPS sertifika güven</span><span class="sxs-lookup"><span data-stu-id="4442b-264">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="4442b-265">Linux (WSL) için Windows alt sistemi HTTPS otomatik olarak imzalanan bir sertifika oluşturur. WSL sertifikasına güvenmek için Windows sertifika deposunu yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="4442b-265">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="4442b-266">Oluşturulan WSL sertifikasını dışarı aktarmak için aşağıdaki komutu çalıştırın: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="4442b-266">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="4442b-267">Bir WSL penceresinde aşağıdaki komutu çalıştırın: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="4442b-267">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="4442b-268">Linux, Windows güvenilir sertifika kullanması için yukarıdaki komut ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4442b-268">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="additional-information"></a><span data-ttu-id="4442b-269">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="4442b-269">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="4442b-270">Apache ile Linux'ta ASP.NET Core barındırın: HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4442b-270">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="4442b-271">Nginx ile Linux'ta ASP.NET Core barındırın: HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4442b-271">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="4442b-272">IIS'de SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="4442b-272">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="4442b-273">OWASP HSTS tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="4442b-273">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
