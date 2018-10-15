---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Bir ASP.NET Core web uygulamasını HTTPS/TLS gerektirir öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325607"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="f8e2d-103">ASP.NET core'da HTTPS'yi zorunlu kılma</span><span class="sxs-lookup"><span data-stu-id="f8e2d-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="f8e2d-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8e2d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f8e2d-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-105">This document shows how to:</span></span>

* <span data-ttu-id="f8e2d-106">Tüm istekler için HTTPS gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="f8e2d-107">Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="f8e2d-108">Hiçbir API, bir istemci ilk istek üzerine hassas verileri göndermesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="f8e2d-109">Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="f8e2d-110">`RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="f8e2d-111">API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="f8e2d-112">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="f8e2d-113">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="f8e2d-114">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="f8e2d-115">400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="f8e2d-116">HTTPS'yi zorunlu</span><span class="sxs-lookup"><span data-stu-id="f8e2d-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f8e2d-117">Tüm üretim ASP.NET Core web uygulamaları çağrısı öneririz:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="f8e2d-118">HTTPS yeniden yönlendirmesi Ara ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) tüm HTTP isteklerini HTTPS için yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="f8e2d-119">[UseHsts](#hsts), HTTP katı Aktarım güvenlik protokolü (HSTS).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="f8e2d-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="f8e2d-120">UseHttpsRedirection</span></span>

<span data-ttu-id="f8e2d-121">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="f8e2d-122">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="f8e2d-123">Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="f8e2d-124">Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) tarafından geçersiz kılınmadığı sürece `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="f8e2d-125">Bağlantıyı önbelleğe almayı kararsız davranışı geliştirme ortamlarında yol açabileceğinden kalıcı yeniden yönlendirmeleri yerine geçici yeniden yönlendirmeleri kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="f8e2d-126">Kullanmanızı öneririz [HSTS](#hsts) yalnızca kaynak güvenli istemcileri için sinyal istekleri uygulamada (yalnızca üretim) gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="f8e2d-127">HTTPS'ye yönlendirmek için bir bağlantı noktası için ara yazılımı kullanılabilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="f8e2d-128">Hiçbir bağlantı noktası varsa, HTTPS için yeniden yönlendirme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="f8e2d-129">HTTPS bağlantı noktası, aşağıdaki yaklaşımlardan birini kullanarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="f8e2d-130">Ayarlama `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="f8e2d-131">Ayarlama `ASPNETCORE_HTTPS_PORT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="f8e2d-132">Bir HTTPS URL'si, geliştirme kümesinde *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="f8e2d-133">İçin bir HTTPS URL'si uç nokta yapılandırma [Kestrel](xref:fundamentals/servers/kestrel) veya [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="f8e2d-134">Kestrel'i veya HTTP.sys genel kullanıma yönelik bir uç sunucusu olarak kullanıldığında, hem de dinlenecek Kestrel veya HTTP.sys yapılandırılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="f8e2d-135">İstemci yeniden yönlendirilmiş burada güvenli bağlantı noktası (genellikle, üretim ve geliştirme 5001 443).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="f8e2d-136">Güvensiz bağlantı noktası (genellikle, üretimde 80) ile 5000 geliştirme.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="f8e2d-137">Güvensiz bağlantı güvenli olmayan bir istek almak ve güvenli bağlantı noktasına yönlendirmek için uygulamanın sırası istemci tarafından erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="f8e2d-138">Ayrıca istemci ve sunucu arasında herhangi bir güvenlik duvarı bağlantı noktaları trafik için açık olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="f8e2d-139">Daha fazla bilgi için [Kestrel uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) veya <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="f8e2d-140">Aşağıdaki vurgulanmış kodu çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="f8e2d-141">Çağırma `AddHttpsRedirection` yalnızca değerlerini değiştirmek gerekli olan `HttpsPort` veya `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="f8e2d-142">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="f8e2d-143">Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) için [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="f8e2d-144">Alanlarını kullanın [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) sınıfı atamaların `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="f8e2d-145">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="f8e2d-146">443 varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-146">The default value is 443.</span></span>

<span data-ttu-id="f8e2d-147">Aşağıdaki mekanizmalardan bağlantı noktasını otomatik olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="f8e2d-148">Ara yazılım aracılığıyla bağlantı noktaları bulabilir [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) zaman aşağıdaki koşullar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="f8e2d-149">Kestrel'i veya HTTP.sys doğrudan HTTPS uç noktaları ile kullanılan (Ayrıca uygulamanın Visual Studio Code'nın hata ayıklayıcısı ile çalıştırma için geçerlidir).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="f8e2d-150">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="f8e2d-151">Visual Studio kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="f8e2d-152">IIS Express, HTTPS etkin sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="f8e2d-153">*launchSettings.json* ayarlar `sslPort` IIS Express için.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="f8e2d-154">Bir uygulama bir ters proxy (örneğin, IIS, IIS Express) çalıştırın, `IServerAddressesFeature` kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="f8e2d-155">Bağlantı noktasını el ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-155">The port must be manually configured.</span></span> <span data-ttu-id="f8e2d-156">Bağlantı noktası olarak değil, istekleri yeniden yönlendirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="f8e2d-157">Bağlantı noktası ayarlayarak yapılandırılabilir [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="f8e2d-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="f8e2d-158">**Anahtar**: https_port</span><span class="sxs-lookup"><span data-stu-id="f8e2d-158">**Key**: https_port</span></span>  
<span data-ttu-id="f8e2d-159">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="f8e2d-159">**Type**: *string*</span></span>  
<span data-ttu-id="f8e2d-160">**Varsayılan**: varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="f8e2d-161">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f8e2d-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f8e2d-162">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (ön ek `ASPNETCORE_` Web ana bilgisayarı kullanırken.)</span><span class="sxs-lookup"><span data-stu-id="f8e2d-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="f8e2d-163">Bağlantı noktası URL'si ile ayarlayarak dolaylı olarak yapılandırılabilir `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="f8e2d-164">Ortam değişkenini sunucusunu yapılandırır ve ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="f8e2d-165">Bağlantı noktası ayarlanırsa:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-165">If no port is set:</span></span>

* <span data-ttu-id="f8e2d-166">İstekleri yeniden yönlendirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="f8e2d-167">Ara yazılım, "yeniden yönlendirme için https bağlantı noktasını belirlemek için başarısız oldu." uyarısı günlüğe kaydeder</span><span class="sxs-lookup"><span data-stu-id="f8e2d-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="f8e2d-168">HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="f8e2d-169">`AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="f8e2d-170">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="f8e2d-171">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="f8e2d-172">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS'yi zorunlu tutmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="f8e2d-173">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri donatmak veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="f8e2d-174">Genel öznitelik uygulamak için aşağıdaki kodu ekleyin. `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="f8e2d-175">Tüm istekleri kullanmak önceki vurgulanan kod gerektirir `HTTPS`; bu nedenle, HTTP isteklerini dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="f8e2d-176">Aşağıdaki vurgulanmış kodu tüm HTTP isteklerini HTTPS için yeniden yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="f8e2d-177">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="f8e2d-178">Ara yazılım Ayrıca, uygulama yeniden yönlendirme yürütüldüğünde, durum kodu veya durum kodunu ve bağlantı noktası ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="f8e2d-179">Genel olarak HTTPS'yi zorunlu (`options.Filters.Add(new RequireHttpsAttribute());`) güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="f8e2d-180">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfaları için öznitelik değil olarak kabul genel HTTPS'yi zorunlu kadar güvenli.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="f8e2d-181">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfaları eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="f8e2d-182">HTTP taşıma katı güvenlik protokolü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="f8e2d-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="f8e2d-183">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="f8e2d-184">Olduğunda bir [HSTS destekleyen bir tarayıcı](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) bu üstbilgiyi alır:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="f8e2d-185">Tarayıcıya HTTP üzerinden herhangi bir iletişim göndermeyi önler etki alanı için yapılandırma depolar.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="f8e2d-186">Tarayıcı tüm iletişim HTTPS üzerinden zorlar.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="f8e2d-187">Tarayıcı kullanıcı, güvenilmeyen ya da geçersiz bir sertifika kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="f8e2d-188">Tarayıcı geçici olarak bu tür bir sertifika güven etmesine istemlerini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="f8e2d-189">İstemci tarafından HSTS zorunlu kılındığından bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="f8e2d-190">İstemci HSTS desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-190">The client must support HSTS.</span></span>
* <span data-ttu-id="f8e2d-191">HSTS HSTS ilkesi oluşturmak üzere en az bir başarılı HTTPS isteğini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="f8e2d-192">Uygulama gerekir her HTTP isteği denetleyin ve yeniden yönlendirme veya HTTP isteği reddedebilir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="f8e2d-193">ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="f8e2d-194">Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="f8e2d-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="f8e2d-195">`UseHsts` HSTS ayarları yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="f8e2d-196">Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="f8e2d-197">HTTPS için ilk kez uygulama, üretim ortamları için başlangıç HSTS değeri küçük bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="f8e2d-198">HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="f8e2d-199">HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="f8e2d-200">Aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="f8e2d-201">Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="f8e2d-202">Önyükleme bölümü olmayan [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="f8e2d-203">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="f8e2d-204">Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="f8e2d-205">Açıkça Strıct aktarım güvenliği üst bilgi, max-age parametresini 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="f8e2d-206">Aksi durumda, 30 gün için varsayılanları ayarlama.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="f8e2d-207">Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="f8e2d-208">Ekler `example.com` dışlanacak konaklar listesine.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="f8e2d-209">`UseHsts` şu geri döngü konakları hariç tutar:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="f8e2d-210">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="f8e2d-211">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="f8e2d-212">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="f8e2d-213">Yukarıdaki örnekte, ek konaklarının nasıl ekleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="f8e2d-214">Çevirme HTTPS/HSTS, proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="f8e2d-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="f8e2d-215">Bağlantı güvenliği ağ genel kullanıma yönelik ucuna nerede işlendiğini bazı arka uç hizmeti senaryolarda, her düğümde bağlantı güvenliği yapılandırma gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="f8e2d-216">Web uygulamaları Visual Studio'da veya gelen şablonlardan oluşturulan [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu etkinleştirme [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="f8e2d-217">Bu senaryolar gerektirmeyen dağıtımları için HTTPS/HSTS şablondan uygulama oluşturulduğunda çevirme.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="f8e2d-218">Çevirme HTTPS/HSTS için:</span><span class="sxs-lookup"><span data-stu-id="f8e2d-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e2d-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e2d-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="f8e2d-220">Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Varlık diyagramı](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f8e2d-222">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f8e2d-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="f8e2d-223">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="f8e2d-223">Use the `--no-https` option.</span></span> <span data-ttu-id="f8e2d-224">Örneğin</span><span class="sxs-lookup"><span data-stu-id="f8e2d-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="f8e2d-225">Docker için bir geliştirici sertifikası ayarlama</span><span class="sxs-lookup"><span data-stu-id="f8e2d-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="f8e2d-226">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="f8e2d-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="f8e2d-227">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="f8e2d-227">Additional information</span></span>

* [<span data-ttu-id="f8e2d-228">OWASP HSTS tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="f8e2d-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
