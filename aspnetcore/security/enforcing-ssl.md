---
title: ASP.NET Core 'de HTTPS 'yi zorla
author: rick-anderson
description: ASP.NET Core Web uygulamasında HTTPS/TLS isteme hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 09/14/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 82cd2e52f3bd929682b9eae24611ad04fd9f8682
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317363"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="68f5d-103">ASP.NET Core 'de HTTPS 'yi zorla</span><span class="sxs-lookup"><span data-stu-id="68f5d-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="68f5d-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="68f5d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="68f5d-105">Bu belgede nasıl yapılacağı gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="68f5d-105">This document shows how to:</span></span>

* <span data-ttu-id="68f5d-106">Tüm istekler için HTTPS gerektir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="68f5d-107">Tüm HTTP isteklerini HTTPS 'ye yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="68f5d-108">Hiçbir API, istemcinin ilk istekte hassas veri göndermesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="68f5d-109">API projeleri</span><span class="sxs-lookup"><span data-stu-id="68f5d-109">API projects</span></span>
>
> <span data-ttu-id="68f5d-110">Hassas bilgileri alan Web API 'Lerinde [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) **kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="68f5d-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="68f5d-111">`RequireHttpsAttribute`, tarayıcıların HTTP 'den HTTPS 'ye yönlendirilmesini sağlamak için HTTP durum kodlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="68f5d-112">API istemcileri HTTP 'den HTTPS 'ye yeniden yönlendirmeyi anlamayabilir veya buna uymayabilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="68f5d-113">Bu tür istemciler, HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="68f5d-114">Web API 'Leri şunlardan biri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="68f5d-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="68f5d-115">HTTP üzerinde dinleme yok.</span><span class="sxs-lookup"><span data-stu-id="68f5d-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="68f5d-116">Durum kodu 400 olan bağlantıyı kapatın (Hatalı Istek) ve isteğe bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="68f5d-117">HSTS ve API projeleri</span><span class="sxs-lookup"><span data-stu-id="68f5d-117">HSTS and API projects</span></span>
>
> <span data-ttu-id="68f5d-118">Varsayılan API projeleri, genellikle yalnızca bir tarayıcı yönergesi olduğu için [HSTS](#hsts) içermez.</span><span class="sxs-lookup"><span data-stu-id="68f5d-118">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="68f5d-119">Telefon veya masaüstü uygulamaları gibi diğer çağıranlar, yönergeye **uymayın.**</span><span class="sxs-lookup"><span data-stu-id="68f5d-119">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="68f5d-120">Tarayıcılar içinde bile, HTTP üzerinden bir API 'ye yapılan tek bir kimlik doğrulamalı çağrı, güvenli olmayan ağlarda risk içerir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-120">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="68f5d-121">Güvenli yaklaşım, API projelerini yalnızca HTTPS dinlemesi ve HTTPS üzerinden yanıt verecek şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-121">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="68f5d-122">API projeleri</span><span class="sxs-lookup"><span data-stu-id="68f5d-122">API projects</span></span>
>
> <span data-ttu-id="68f5d-123">Hassas bilgileri alan Web API 'Lerinde [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) **kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="68f5d-123">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="68f5d-124">`RequireHttpsAttribute`, tarayıcıların HTTP 'den HTTPS 'ye yönlendirilmesini sağlamak için HTTP durum kodlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-124">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="68f5d-125">API istemcileri HTTP 'den HTTPS 'ye yeniden yönlendirmeyi anlamayabilir veya buna uymayabilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-125">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="68f5d-126">Bu tür istemciler, HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-126">Such clients may send information over HTTP.</span></span> <span data-ttu-id="68f5d-127">Web API 'Leri şunlardan biri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="68f5d-127">Web APIs should either:</span></span>
>
> * <span data-ttu-id="68f5d-128">HTTP üzerinde dinleme yok.</span><span class="sxs-lookup"><span data-stu-id="68f5d-128">Not listen on HTTP.</span></span>
> * <span data-ttu-id="68f5d-129">Durum kodu 400 olan bağlantıyı kapatın (Hatalı Istek) ve isteğe bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-129">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="68f5d-130">HTTPS gerektir</span><span class="sxs-lookup"><span data-stu-id="68f5d-130">Require HTTPS</span></span>

<span data-ttu-id="68f5d-131">Üretim ASP.NET Core Web uygulamalarının kullanmasını öneririz:</span><span class="sxs-lookup"><span data-stu-id="68f5d-131">We recommend that production ASP.NET Core web apps use:</span></span>

* <span data-ttu-id="68f5d-132">HTTP isteklerini HTTPS 'ye yeniden yönlendirmek için HTTPS yeniden yönlendirme ara yazılımı (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>).</span><span class="sxs-lookup"><span data-stu-id="68f5d-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="68f5d-133">İstemcilere HTTP katı aktarım güvenliği Protokolü (HSTS) üst bilgileri göndermek için HSTS ara yazılımı ([Usehsts](#http-strict-transport-security-protocol-hsts)).</span><span class="sxs-lookup"><span data-stu-id="68f5d-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="68f5d-134">Ters Proxy yapılandırmasında dağıtılan uygulamalar, proxy 'nin bağlantı güvenliğini (HTTPS) işlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="68f5d-135">Ara sunucu HTTPS yeniden yönlendirmeyi de işleiyorsa, HTTPS yeniden yönlendirme ara yazılımı kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="68f5d-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="68f5d-136">Proxy sunucusu Ayrıca, HSTS üst bilgilerini (örneğin, [ııs 10,0 (1709) veya sonraki sürümlerde yerel HSTS desteği](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)) yazmayı de işişişişsa, uygulama Için HSTS ara yazılımı gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="68f5d-137">Daha fazla bilgi için bkz. [Proje oluşturma SıRASıNDA https/HSTS 'nin geri çevirme](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="68f5d-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="68f5d-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="68f5d-138">UseHttpsRedirection</span></span>

<span data-ttu-id="68f5d-139">Aşağıdaki kod `Startup` sınıfında `UseHttpsRedirection` çağırır:</span><span class="sxs-lookup"><span data-stu-id="68f5d-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

<span data-ttu-id="68f5d-140">Vurgulanan önceki kod:</span><span class="sxs-lookup"><span data-stu-id="68f5d-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="68f5d-141">Varsayılan [Httpsredirectionoptions. RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)) kullanır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="68f5d-142">`ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [ıveraddressesözelliği](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)tarafından geçersiz kılınmadıkça varsayılan [Httpsredirectionoptions. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) kullanır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="68f5d-143">Kalıcı yeniden yönlendirmeler yerine geçici yeniden yönlendirmeler kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="68f5d-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="68f5d-144">Bağlantıyı önbelleğe alma, geliştirme ortamlarında kararsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="68f5d-145">Uygulama geliştirme dışı bir ortamda olduğunda kalıcı yeniden yönlendirme durum kodu göndermek isterseniz, [üretim içindeki kalıcı yeniden yönlendirmeleri yapılandırma](#configure-permanent-redirects-in-production) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="68f5d-146">İstemcilere yalnızca güvenli kaynak isteği gönderilmesi gerektiğini bildirmek için [HSTS](#http-strict-transport-security-protocol-hsts) kullanılması önerilir (yalnızca üretimde).</span><span class="sxs-lookup"><span data-stu-id="68f5d-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="68f5d-147">Bağlantı noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="68f5d-147">Port configuration</span></span>

<span data-ttu-id="68f5d-148">Ara yazılımın güvenli olmayan bir isteği HTTPS 'ye yönlendirmesi için bir bağlantı noktası kullanılabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="68f5d-149">Kullanılabilir bağlantı noktası yoksa:</span><span class="sxs-lookup"><span data-stu-id="68f5d-149">If no port is available:</span></span>

* <span data-ttu-id="68f5d-150">HTTPS olarak yeniden yönlendirme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="68f5d-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="68f5d-151">Ara yazılım "yeniden yönlendirme için HTTPS bağlantı noktası saptanamadı" uyarısını günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="68f5d-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="68f5d-152">Aşağıdaki yaklaşımlardan herhangi birini kullanarak HTTPS bağlantı noktasını belirtin:</span><span class="sxs-lookup"><span data-stu-id="68f5d-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="68f5d-153">[Httpsredirectionoptions. HttpsPort](#options)öğesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="68f5d-154">`https_port` [ana bilgisayar ayarını](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port)ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="68f5d-154">Set the `https_port` [host setting](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port):</span></span>

  * <span data-ttu-id="68f5d-155">Konak yapılandırmasında.</span><span class="sxs-lookup"><span data-stu-id="68f5d-155">In host configuration.</span></span>
  * <span data-ttu-id="68f5d-156">`ASPNETCORE_HTTPS_PORT` ortam değişkenini ayarlayarak.</span><span class="sxs-lookup"><span data-stu-id="68f5d-156">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="68f5d-157">*AppSettings. JSON*içine bir üst düzey girişi ekleyerek:</span><span class="sxs-lookup"><span data-stu-id="68f5d-157">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* <span data-ttu-id="68f5d-158">[ASPNETCORE_URLS ortam değişkenini](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls)kullanarak güvenli düzene sahip bir bağlantı noktası belirtin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-158">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span></span> <span data-ttu-id="68f5d-159">Ortam değişkeni sunucusunu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-159">The environment variable configures the server.</span></span> <span data-ttu-id="68f5d-160">Ara yazılım, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>aracılığıyla HTTPS bağlantı noktasını dolaylı olarak bulur.</span><span class="sxs-lookup"><span data-stu-id="68f5d-160">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="68f5d-161">Bu yaklaşım, ters proxy dağıtımlarında çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="68f5d-161">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* <span data-ttu-id="68f5d-162">`https_port` [ana bilgisayar ayarını](xref:fundamentals/host/web-host#https-port)ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="68f5d-162">Set the `https_port` [host setting](xref:fundamentals/host/web-host#https-port):</span></span>

  * <span data-ttu-id="68f5d-163">Konak yapılandırmasında.</span><span class="sxs-lookup"><span data-stu-id="68f5d-163">In host configuration.</span></span>
  * <span data-ttu-id="68f5d-164">`ASPNETCORE_HTTPS_PORT` ortam değişkenini ayarlayarak.</span><span class="sxs-lookup"><span data-stu-id="68f5d-164">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="68f5d-165">*AppSettings. JSON*içine bir üst düzey girişi ekleyerek:</span><span class="sxs-lookup"><span data-stu-id="68f5d-165">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* <span data-ttu-id="68f5d-166">[ASPNETCORE_URLS ortam değişkenini](xref:fundamentals/host/web-host#server-urls)kullanarak güvenli düzene sahip bir bağlantı noktası belirtin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-166">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](xref:fundamentals/host/web-host#server-urls).</span></span> <span data-ttu-id="68f5d-167">Ortam değişkeni sunucusunu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-167">The environment variable configures the server.</span></span> <span data-ttu-id="68f5d-168">Ara yazılım, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>aracılığıyla HTTPS bağlantı noktasını dolaylı olarak bulur.</span><span class="sxs-lookup"><span data-stu-id="68f5d-168">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="68f5d-169">Bu yaklaşım, ters proxy dağıtımlarında çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="68f5d-169">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

* <span data-ttu-id="68f5d-170">Geliştirme aşamasında, *launchsettings. JSON*dosyasında bir https URL 'si ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-170">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="68f5d-171">IIS Express kullanıldığında HTTPS 'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-171">Enable HTTPS when IIS Express is used.</span></span>

* <span data-ttu-id="68f5d-172">[Kestrel](xref:fundamentals/servers/kestrel) Server veya [http. sys](xref:fundamentals/servers/httpsys) sunucusu için genel kullanıma yönelik BIR uç dağıtımı için https URL uç noktası yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-172">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="68f5d-173">Uygulama tarafından yalnızca **BIR HTTPS bağlantı noktası** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-173">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="68f5d-174">Ara yazılım, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>aracılığıyla bağlantı noktasını bulur.</span><span class="sxs-lookup"><span data-stu-id="68f5d-174">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="68f5d-175">Bir uygulama ters proxy yapılandırmasında çalıştırıldığında <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="68f5d-175">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="68f5d-176">Bu bölümde açıklanan diğer yaklaşımlardan birini kullanarak bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-176">Set the port using one of the other approaches described in this section.</span></span>

### <a name="edge-deployments"></a><span data-ttu-id="68f5d-177">Edge dağıtımları</span><span class="sxs-lookup"><span data-stu-id="68f5d-177">Edge deployments</span></span> 

<span data-ttu-id="68f5d-178">Kestrel veya HTTP. sys, herkese açık bir uç sunucu olarak kullanıldığında, Kestrel veya HTTP. sys ' nin her ikisini de dinlemek üzere yapılandırılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="68f5d-178">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="68f5d-179">İstemcinin yeniden yönlendirildiği güvenli bağlantı noktası (genellikle üretim ortamında 443 ve geliştirme sırasında 5001).</span><span class="sxs-lookup"><span data-stu-id="68f5d-179">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="68f5d-180">Güvenli olmayan bağlantı noktası (genellikle üretim ortamında 80 ve geliştirme sırasında 5000).</span><span class="sxs-lookup"><span data-stu-id="68f5d-180">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="68f5d-181">Güvenli olmayan bir istek alabilmesi ve istemciyi güvenli bağlantı noktasına yeniden yönlendirebilmesi için güvensiz bağlantı noktasına istemci tarafından erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-181">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="68f5d-182">Daha fazla bilgi için bkz. [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) veya <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="68f5d-182">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="68f5d-183">Dağıtım senaryoları</span><span class="sxs-lookup"><span data-stu-id="68f5d-183">Deployment scenarios</span></span>

<span data-ttu-id="68f5d-184">İstemci ve sunucu arasındaki herhangi bir güvenlik duvarının trafik için iletişim bağlantı noktaları açık olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-184">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="68f5d-185">İstekler ters bir ara sunucu yapılandırmasında iletilirse, HTTPS yeniden yönlendirme ara yazılımı çağrılmadan önce [Iletilen üstbilgiler ara yazılımını](xref:host-and-deploy/proxy-load-balancer) kullanın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-185">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="68f5d-186">İletilen üstbilgiler ara yazılımı, `X-Forwarded-Proto` üst bilgisini kullanarak `Request.Scheme`güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-186">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="68f5d-187">Ara yazılım, yeniden yönlendirme URI 'Leri ve diğer güvenlik ilkelerinin doğru çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-187">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="68f5d-188">Iletilen üstbilgiler ara yazılımı kullanılmazsa, arka uç uygulaması doğru düzeni alamayabilir ve yeniden yönlendirme döngüsünde sona ermeyebilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-188">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="68f5d-189">Yaygın bir son kullanıcı hata iletisi, çok fazla yeniden yönlendirme meydana geldi.</span><span class="sxs-lookup"><span data-stu-id="68f5d-189">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="68f5d-190">Azure App Service dağıtım sırasında, [öğreticideki yönergeleri izleyin: mevcut bir özel SSL sertifikasını Azure 'A bağlama Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="68f5d-190">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="68f5d-191">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="68f5d-191">Options</span></span>

<span data-ttu-id="68f5d-192">Aşağıdaki vurgulanan kod, ara yazılım seçeneklerini yapılandırmak için [Addhttpsredirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) öğesini çağırır:</span><span class="sxs-lookup"><span data-stu-id="68f5d-192">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


<span data-ttu-id="68f5d-193">`AddHttpsRedirection` çağırmak için yalnızca `HttpsPort` veya `RedirectStatusCode`değerlerinin değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-193">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="68f5d-194">Vurgulanan önceki kod:</span><span class="sxs-lookup"><span data-stu-id="68f5d-194">The preceding highlighted code:</span></span>

* <span data-ttu-id="68f5d-195">[Httpsredirectionoptions. RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) değerini, varsayılan değer olan <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>belirler.</span><span class="sxs-lookup"><span data-stu-id="68f5d-195">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="68f5d-196">`RedirectStatusCode`atamaları için <xref:Microsoft.AspNetCore.Http.StatusCodes> sınıfının alanlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-196">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="68f5d-197">HTTPS bağlantı noktasını 5001 olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-197">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="68f5d-198">Varsayılan değer 443 ' dir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-198">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="68f5d-199">Üretimde kalıcı yeniden yönlendirmeleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="68f5d-199">Configure permanent redirects in production</span></span>

<span data-ttu-id="68f5d-200">Ara yazılım varsayılan olarak tüm yeniden yönlendirmelere bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) gönderir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-200">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="68f5d-201">Uygulama geliştirme dışı bir ortamda olduğunda kalıcı yeniden yönlendirme durum kodu göndermek isterseniz, geliştirme dışı bir ortam için bir koşullu denetim içindeki ara yazılım seçenekleri yapılandırmasını sarın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-201">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68f5d-202">*Startup.cs*'de Hizmetleri yapılandırırken:</span><span class="sxs-lookup"><span data-stu-id="68f5d-202">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
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

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="68f5d-203">*Startup.cs*'de Hizmetleri yapılandırırken:</span><span class="sxs-lookup"><span data-stu-id="68f5d-203">When configuring services in *Startup.cs*:</span></span>

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

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="68f5d-204">HTTPS yeniden yönlendirme ara yazılımı alternatif yaklaşımı</span><span class="sxs-lookup"><span data-stu-id="68f5d-204">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="68f5d-205">HTTPS yeniden yönlendirme ara yazılımı (`UseHttpsRedirection`) kullanmanın bir alternatifi URL yeniden yazma ara yazılımı (`AddRedirectToHttps`) kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-205">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="68f5d-206">`AddRedirectToHttps`, yeniden yönlendirme yürütüldüğünde durum kodunu ve bağlantı noktasını da ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-206">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="68f5d-207">Daha fazla bilgi için bkz. [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="68f5d-207">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="68f5d-208">Ek yeniden yönlendirme kuralları gereksinimi olmadan HTTPS 'ye yönlendirilirken, bu konuda açıklanan HTTPS yeniden yönlendirme ara yazılımı (`UseHttpsRedirection`) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="68f5d-208">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="68f5d-209">HTTP katı aktarım güvenliği Protokolü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="68f5d-209">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="68f5d-210">Her [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)için, [http katı aktarım güvenliği (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) , yanıt üst bilgisi kullanılarak bir Web uygulaması tarafından belirtilen bir katılım güvenlik geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-210">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="68f5d-211">[HSTS 'yi destekleyen bir tarayıcı](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) bu üstbilgiyi alırsa:</span><span class="sxs-lookup"><span data-stu-id="68f5d-211">When a [browser that supports HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) receives this header:</span></span>

* <span data-ttu-id="68f5d-212">Tarayıcı, HTTP üzerinden iletişimin gönderilmesini önleyen etki alanı için yapılandırmayı depolar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-212">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="68f5d-213">Tarayıcı, HTTPS üzerinden tüm iletişimi zorlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-213">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="68f5d-214">Tarayıcı, kullanıcının güvenilmeyen veya geçersiz sertifikalar kullanmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="68f5d-214">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="68f5d-215">Tarayıcı, kullanıcının bu sertifikaya geçici olarak güvenmesine izin veren istemleri devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-215">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="68f5d-216">HSTS istemci tarafından zorlandığından bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="68f5d-216">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="68f5d-217">İstemcinin HSTS 'yi desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-217">The client must support HSTS.</span></span>
* <span data-ttu-id="68f5d-218">HSTS, HSTS ilkesini oluşturmak için en az bir başarılı HTTPS isteği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-218">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="68f5d-219">Uygulamanın her HTTP isteğini denetlemesi ve HTTP isteğini yeniden yönlendirmesi veya reddetmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-219">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="68f5d-220">ASP.NET Core 2,1 ve üzeri `UseHsts` genişletme yöntemiyle HSTS uygular.</span><span class="sxs-lookup"><span data-stu-id="68f5d-220">ASP.NET Core 2.1 and later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="68f5d-221">Aşağıdaki kod, uygulama [geliştirme modunda](xref:fundamentals/environments)olmadığında `UseHsts` çağırır:</span><span class="sxs-lookup"><span data-stu-id="68f5d-221">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

<span data-ttu-id="68f5d-222">`UseHsts` geliştirmede önerilmez çünkü HSTS ayarları tarayıcılar tarafından yüksek oranda önbelleğe alınabilir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-222">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="68f5d-223">Varsayılan olarak, `UseHsts` yerel geri döngü adresini dışlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-223">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="68f5d-224">İlk kez HTTPS 'yi uygulayan üretim ortamları için, <xref:System.TimeSpan> yöntemlerinden birini kullanarak ilk [HstsOptions. maxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) değerini küçük bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-224">For production environments that are implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="68f5d-225">HTTPS altyapısını HTTP 'ye döndürmeniz gerekirse, değeri saat olarak bir tek güne kadar ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-225">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="68f5d-226">HTTPS yapılandırmasının sürdürülebilirliği konusunda emin olduktan sonra, HSTS maksimum yaş değerini artırın; yaygın olarak kullanılan bir değer bir yıldır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-226">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="68f5d-227">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="68f5d-227">The following code:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* <span data-ttu-id="68f5d-228">Strict-Transport-Security üstbilgisinin preload parametresini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-228">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="68f5d-229">Önyükleme, [RFC HSTS belirtiminin](https://tools.ietf.org/html/rfc6797)bir parçası değildir, ancak Web tarayıcıları tarafından Yeni yüklemede HSTS sitelerini önceden yüklemek için desteklenir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-229">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="68f5d-230">Daha fazla bilgi için bkz. [https://hstspreload.org/](https://hstspreload.org/) .</span><span class="sxs-lookup"><span data-stu-id="68f5d-230">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="68f5d-231">SSTS ilkesini konak alt etki alanlarını barındıracak şekilde uygulayan [ıncludealt etki alanını](https://tools.ietf.org/html/rfc6797#section-6.1.2)sağlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-231">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="68f5d-232">Strict-Transport-Security üstbilgisinin Max-Age parametresini açıkça 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-232">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="68f5d-233">Ayarlanmazsa, varsayılan olarak 30 gün olur.</span><span class="sxs-lookup"><span data-stu-id="68f5d-233">If not set, defaults to 30 days.</span></span> <span data-ttu-id="68f5d-234">Daha fazla bilgi için bkz. [Maksimum yaş yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) .</span><span class="sxs-lookup"><span data-stu-id="68f5d-234">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="68f5d-235">Dışlanacak konaklar listesine `example.com` ekler.</span><span class="sxs-lookup"><span data-stu-id="68f5d-235">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="68f5d-236">`UseHsts` aşağıdaki geri döngü Konakları dışlar:</span><span class="sxs-lookup"><span data-stu-id="68f5d-236">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="68f5d-237">`localhost`: IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="68f5d-237">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="68f5d-238">`127.0.0.1`: IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="68f5d-238">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="68f5d-239">`[::1]`: IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="68f5d-239">`[::1]` : The IPv6 loopback address.</span></span>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="68f5d-240">Proje oluşturulurken HTTPS/HSTS 'nin katılımı</span><span class="sxs-lookup"><span data-stu-id="68f5d-240">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="68f5d-241">Bağlantı güvenliğinin ağın herkese açık kenarından işlendiği bazı arka uç hizmet senaryolarında, her düğümde bağlantı güvenliğini yapılandırmak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-241">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="68f5d-242">Visual Studio 'da veya [DotNet yeni](/dotnet/core/tools/dotnet-new) komutundan oluşturulan Web Apps, [https yeniden yönlendirmeyi](#require-https) ve [HSTS](#http-strict-transport-security-protocol-hsts)'yi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-242">Web apps that are generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="68f5d-243">Bu senaryoları gerektirmeyen dağıtımlar için, uygulama şablondan oluşturulduğunda HTTPS/HSTS 'nin devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68f5d-243">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="68f5d-244">HTTPS/HSTS 'yi devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="68f5d-244">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68f5d-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68f5d-245">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="68f5d-246">**Https Için Yapılandır** onay kutusunun işaretini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-246">Uncheck the **Configure for HTTPS** check box.</span></span>

::: moniker range=">= aspnetcore-3.0"

![HTTPS için Yapılandır onay kutusunun seçili olduğu yeni ASP.NET Core Web uygulaması iletişim kutusu.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![HTTPS için Yapılandır onay kutusunun seçili olduğu yeni ASP.NET Core Web uygulaması iletişim kutusu.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="68f5d-249">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="68f5d-249">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="68f5d-250">`--no-https` seçeneğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-250">Use the `--no-https` option.</span></span> <span data-ttu-id="68f5d-251">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="68f5d-251">For example</span></span>

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="68f5d-252">Windows ve macOS 'ta ASP.NET Core HTTPS geliştirme sertifikasına güvenin</span><span class="sxs-lookup"><span data-stu-id="68f5d-252">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="68f5d-253">.NET Core SDK bir HTTPS geliştirme sertifikası içerir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-253">The .NET Core SDK includes an HTTPS development certificate.</span></span> <span data-ttu-id="68f5d-254">Sertifika, ilk çalıştırma deneyiminin bir parçası olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-254">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="68f5d-255">Örneğin, `dotnet --info` aşağıdakine benzer bir çıktı üretir:</span><span class="sxs-lookup"><span data-stu-id="68f5d-255">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="68f5d-256">.NET Core SDK yükleme ASP.NET Core HTTPS geliştirme sertifikasını Yerel Kullanıcı sertifika deposuna yüklenir.</span><span class="sxs-lookup"><span data-stu-id="68f5d-256">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="68f5d-257">Sertifika yüklendi, ancak güvenilir değil.</span><span class="sxs-lookup"><span data-stu-id="68f5d-257">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="68f5d-258">Sertifikaya güvenmek için, DotNet `dev-certs` aracını çalıştırmak için tek seferlik bir adım gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="68f5d-258">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="68f5d-259">Aşağıdaki komut `dev-certs` aracında yardım sağlar:</span><span class="sxs-lookup"><span data-stu-id="68f5d-259">The following command provides help on the `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="68f5d-260">Docker için Geliştirici Sertifikası ayarlama</span><span class="sxs-lookup"><span data-stu-id="68f5d-260">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="68f5d-261">[Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/6199)bakın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-261">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="68f5d-262">Linux için Windows alt sistemi 'nden HTTPS sertifikasına güven</span><span class="sxs-lookup"><span data-stu-id="68f5d-262">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="68f5d-263">Linux için Windows alt sistemi (WSL), HTTPS otomatik olarak imzalanan bir sertifika oluşturur. Windows sertifika deposunu, WSL sertifikasına güvenmek üzere yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="68f5d-263">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="68f5d-264">WSL tarafından oluşturulan sertifikayı dışarı aktarmak için şu komutu çalıştırın: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="68f5d-264">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="68f5d-265">Bir WSL penceresinde, şu komutu çalıştırın: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="68f5d-265">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="68f5d-266">Yukarıdaki komut, ortam değişkenlerini, Linux 'un Windows güvenilen sertifikasını kullanmasını sağlayacak şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="68f5d-266">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="troubleshoot-certificate-problems"></a><span data-ttu-id="68f5d-267">Sertifika sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="68f5d-267">Troubleshoot certificate problems</span></span>

<span data-ttu-id="68f5d-268">Bu bölümde, ASP.NET Core HTTPS geliştirme sertifikası [yüklendiğinde ve güveniliyorsa](#trust), ancak hala sertifikaya güvenilmediğini belirten tarayıcı uyarıları varsa yardım sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-268">This section provides help when the ASP.NET Core HTTPS development certificate has been [installed and trusted](#trust), but you still have browser warnings that the certificate is not trusted.</span></span> <span data-ttu-id="68f5d-269">ASP.NET Core HTTPS geliştirme sertifikası, [Kestrel](xref:fundamentals/servers/kestrel)tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-269">The ASP.NET Core HTTPS development certificate is used by [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

### <a name="all-platforms---certificate-not-trusted"></a><span data-ttu-id="68f5d-270">Tüm platformlar-Sertifikaya güvenilmiyor</span><span class="sxs-lookup"><span data-stu-id="68f5d-270">All platforms - certificate not trusted</span></span>

<span data-ttu-id="68f5d-271">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="68f5d-271">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="68f5d-272">Açık olan tüm tarayıcı örneklerini kapatın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-272">Close any browser instances open.</span></span> <span data-ttu-id="68f5d-273">Uygulamaya yeni bir tarayıcı penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-273">Open a new browser window to app.</span></span> <span data-ttu-id="68f5d-274">Sertifika güveni tarayıcılar tarafından önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="68f5d-274">Certificate trust is cached by browsers.</span></span>

<span data-ttu-id="68f5d-275">Yukarıdaki komutlar çoğu tarayıcı güveni sorununu çözüyor.</span><span class="sxs-lookup"><span data-stu-id="68f5d-275">The preceding commands solve most browser trust issues.</span></span> <span data-ttu-id="68f5d-276">Tarayıcı yine de sertifikaya güvenmiyor ise, aşağıdaki platforma özgü önerileri izleyin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-276">If the browser is still not trusting the certificate, follow the platform specific suggestions that follow.</span></span>

### <a name="docker---certificate-not-trusted"></a><span data-ttu-id="68f5d-277">Docker-Sertifikaya güvenilmiyor</span><span class="sxs-lookup"><span data-stu-id="68f5d-277">Docker - certificate not trusted</span></span>

* <span data-ttu-id="68f5d-278">*C:\Users\{User} \AppData\Roaming\ASP.NET\Https* klasörünü silin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-278">Delete the *C:\Users\{USER}\AppData\Roaming\ASP.NET\Https* folder.</span></span>
* <span data-ttu-id="68f5d-279">Çözümü temizleyin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-279">Clean the solution.</span></span> <span data-ttu-id="68f5d-280">*Bin* ve *obj* klasörlerini silin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-280">Delete the *bin* and *obj* folders.</span></span>
* <span data-ttu-id="68f5d-281">Geliştirme aracını yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-281">Restart the development tool.</span></span> <span data-ttu-id="68f5d-282">Örneğin, Visual Studio, Visual Studio Code veya Mac için Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68f5d-282">For example, Visual Studio, Visual Studio Code, or Visual Studio for Mac.</span></span>

### <a name="windows---certificate-not-trusted"></a><span data-ttu-id="68f5d-283">Windows-Sertifikaya güvenilmiyor</span><span class="sxs-lookup"><span data-stu-id="68f5d-283">Windows - certificate not trusted</span></span>

* <span data-ttu-id="68f5d-284">Sertifika deposundaki sertifikaları kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-284">Check the certificates in the certificate store.</span></span> <span data-ttu-id="68f5d-285">`ASP.NET Core HTTPS development certificate` kolay ada sahip `localhost` bir sertifika olması gerekir `Current User > Personal > Certificates` ve `Current User > Trusted root certification authorities > Certificates`</span><span class="sxs-lookup"><span data-stu-id="68f5d-285">There should be a `localhost` certificate with the `ASP.NET Core HTTPS development certificate` friendly name both under `Current User > Personal > Certificates` and `Current User > Trusted root certification authorities > Certificates`</span></span>
* <span data-ttu-id="68f5d-286">Yalnızca kişisel ve güvenilen kök sertifika yetkililerinden bulunan tüm sertifikaları kaldırın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-286">Remove all the found certificates from both Personal and Trusted root certification authorities.</span></span> <span data-ttu-id="68f5d-287">IIS Express localhost **sertifikasını kaldırmayın.**</span><span class="sxs-lookup"><span data-stu-id="68f5d-287">Do **not** remove the IIS Express localhost certificate.</span></span>
* <span data-ttu-id="68f5d-288">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="68f5d-288">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="68f5d-289">Açık olan tüm tarayıcı örneklerini kapatın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-289">Close any browser instances open.</span></span> <span data-ttu-id="68f5d-290">Uygulamaya yeni bir tarayıcı penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-290">Open a new browser window to app.</span></span>

### <a name="os-x---certificate-not-trusted"></a><span data-ttu-id="68f5d-291">OS X-Sertifikaya güvenilmiyor</span><span class="sxs-lookup"><span data-stu-id="68f5d-291">OS X - certificate not trusted</span></span>

* <span data-ttu-id="68f5d-292">Anahtarlık erişimini açın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-292">Open KeyChain Access.</span></span>
* <span data-ttu-id="68f5d-293">Sistem anahtarlığı ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-293">Select the System keychain.</span></span>
* <span data-ttu-id="68f5d-294">Localhost sertifikası olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-294">Check for the presence of a localhost certificate.</span></span>
* <span data-ttu-id="68f5d-295">Tüm kullanıcılar için güvenilir olduğunu göstermek için simgenin üzerinde bir `+` simgesi içerip içermediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="68f5d-295">Check that it contains a `+` symbol on the icon to indicate its trusted for all users.</span></span>
* <span data-ttu-id="68f5d-296">Sertifikayı sistem anahtarlığınızdan kaldırın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-296">Remove the certificate from the system keychain.</span></span>
* <span data-ttu-id="68f5d-297">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="68f5d-297">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="68f5d-298">Açık olan tüm tarayıcı örneklerini kapatın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-298">Close any browser instances open.</span></span> <span data-ttu-id="68f5d-299">Uygulamaya yeni bir tarayıcı penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-299">Open a new browser window to app.</span></span>

<span data-ttu-id="68f5d-300">Visual Studio 'da sertifika sorunlarını gidermek için [IIS Express (ASPNET/AspNetCore #16892) kullanarak https hatası](https://github.com/aspnet/AspNetCore/issues/16892) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="68f5d-300">See [HTTPS Error using IIS Express (aspnet/AspNetCore #16892)](https://github.com/aspnet/AspNetCore/issues/16892) for troubleshooting certificate issues with Visual Studio.</span></span>

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a><span data-ttu-id="68f5d-301">Visual Studio ile kullanılan SSL sertifikası IIS Express</span><span class="sxs-lookup"><span data-stu-id="68f5d-301">IIS Express SSL certificate used with Visual Studio</span></span>

<span data-ttu-id="68f5d-302">IIS Express sertifikayla ilgili sorunları gidermek için Visual Studio yükleyicisinden **Onar** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="68f5d-302">To fix problems with the IIS Express certificate, select **Repair** from the Visual Studio installer.</span></span>

## <a name="additional-information"></a><span data-ttu-id="68f5d-303">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="68f5d-303">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="68f5d-304">Linux üzerinde Apache: HTTPS yapılandırmasıyla konak ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68f5d-304">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="68f5d-305">NGINX ile Linux üzerinde ana bilgisayar ASP.NET Core: HTTPS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="68f5d-305">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="68f5d-306">IIS 'de SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="68f5d-306">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="68f5d-307">OWASP HSTS tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="68f5d-307">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
