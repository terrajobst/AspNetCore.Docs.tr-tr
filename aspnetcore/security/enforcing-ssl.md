---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Web uygulaması nasıl bir ASP.NET Core HTTPS/TLS'ye gerektirecek şekilde gösterir.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 06ff89d30fb3586c69274cc7cb3e6c75065b098a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011332"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="2030d-103">ASP.NET core'da HTTPS'yi zorunlu kılma</span><span class="sxs-lookup"><span data-stu-id="2030d-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="2030d-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2030d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2030d-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="2030d-105">This document shows how to:</span></span>

* <span data-ttu-id="2030d-106">Tüm istekler için HTTPS gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2030d-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="2030d-107">Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="2030d-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="2030d-108">Hiçbir API, bir istemci ilk istek üzerine hassas verileri göndermesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="2030d-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="2030d-109">Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="2030d-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="2030d-110">`RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="2030d-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="2030d-111">API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="2030d-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="2030d-112">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="2030d-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="2030d-113">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="2030d-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="2030d-114">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="2030d-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="2030d-115">400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.</span><span class="sxs-lookup"><span data-stu-id="2030d-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="2030d-116">HTTPS'yi zorunlu</span><span class="sxs-lookup"><span data-stu-id="2030d-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2030d-117">Tüm üretim ASP.NET Core web uygulamaları çağrısı öneririz:</span><span class="sxs-lookup"><span data-stu-id="2030d-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="2030d-118">HTTPS yeniden yönlendirmesi Ara ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) tüm HTTP isteklerini HTTPS için yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="2030d-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="2030d-119">[UseHsts](#hsts), HTTP katı Aktarım güvenlik protokolü (HSTS).</span><span class="sxs-lookup"><span data-stu-id="2030d-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="2030d-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="2030d-120">UseHttpsRedirection</span></span>

<span data-ttu-id="2030d-121">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="2030d-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="2030d-122">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="2030d-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="2030d-123">Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="2030d-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="2030d-124">Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) tarafından geçersiz kılınmadığı sürece `ASPNETCORE_HTTPS_PORT` ortam değişkeni veya [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="2030d-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="2030d-125">HTTPS'ye yönlendirmek için bir bağlantı noktası için ara yazılımı kullanılabilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2030d-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="2030d-126">Bağlantı noktası varsa, HTTPS için yeniden yönlendirme gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="2030d-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="2030d-127">HTTPS bağlantı noktası aşağıdaki ayarı tarafından belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="2030d-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="2030d-128">`ASPNETCORE_HTTPS_PORT` Ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2030d-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="2030d-129">Geliştirme, içinde bir HTTPS URL'si *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2030d-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="2030d-130">Doğrudan Kestrel veya HttpSys üzerinde yapılandırılan bir HTTPS URL'si.</span><span class="sxs-lookup"><span data-stu-id="2030d-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="2030d-131">Aşağıdaki vurgulanmış kodu çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="2030d-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="2030d-132">Çağırma `AddHttpsRedirection` yalnızca değerlerini değiştirmek gerekli olan ` HttpsPort` veya ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="2030d-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="2030d-133">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="2030d-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="2030d-134">Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) için `Status307TemporaryRedirect`, varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="2030d-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="2030d-135">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2030d-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="2030d-136">443 varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="2030d-136">The default value is 443.</span></span>

<span data-ttu-id="2030d-137">Aşağıdaki mekanizmalardan bağlantı noktasını otomatik olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2030d-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="2030d-138">Ara yazılım aracılığıyla bağlantı noktaları bulabilir [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) zaman aşağıdaki koşullar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="2030d-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="2030d-139">Kestrel'i veya HTTP.sys doğrudan HTTPS uç noktaları ile kullanılan (Ayrıca uygulamanın Visual Studio Code'nın hata ayıklayıcısı ile çalıştırma için geçerlidir).</span><span class="sxs-lookup"><span data-stu-id="2030d-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="2030d-140">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2030d-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="2030d-141">Visual Studio kullanılır:</span><span class="sxs-lookup"><span data-stu-id="2030d-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="2030d-142">IIS Express, HTTPS etkin sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2030d-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="2030d-143">*launchSettings.json* ayarlar `sslPort` IIS Express için.</span><span class="sxs-lookup"><span data-stu-id="2030d-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="2030d-144">Bir uygulama bir ters proxy (örneğin, IIS, IIS Express) çalıştırın, `IServerAddressesFeature` kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="2030d-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="2030d-145">Bağlantı noktasını el ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2030d-145">The port must be manually configured.</span></span> <span data-ttu-id="2030d-146">Bağlantı noktası olarak değil, istekleri yeniden yönlendirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="2030d-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="2030d-147">Bağlantı noktası ayarlayarak yapılandırılabilir [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="2030d-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="2030d-148">**Anahtar**: https_port</span><span class="sxs-lookup"><span data-stu-id="2030d-148">**Key**: https_port</span></span>  
<span data-ttu-id="2030d-149">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="2030d-149">**Type**: *string*</span></span>  
<span data-ttu-id="2030d-150">**Varsayılan**: varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="2030d-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="2030d-151">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2030d-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="2030d-152">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (ön ek `ASPNETCORE_` Web ana bilgisayarı kullanırken.)</span><span class="sxs-lookup"><span data-stu-id="2030d-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="2030d-153">Bağlantı noktası URL'si ile ayarlayarak dolaylı olarak yapılandırılabilir `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2030d-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="2030d-154">Ortam değişkenini sunucusunu yapılandırır ve ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="2030d-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="2030d-155">Bağlantı noktası ayarlanırsa:</span><span class="sxs-lookup"><span data-stu-id="2030d-155">If no port is set:</span></span>

* <span data-ttu-id="2030d-156">İstekleri yeniden yönlendirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="2030d-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="2030d-157">Ara yazılım, "yeniden yönlendirme için https bağlantı noktasını belirlemek için başarısız oldu." uyarısı günlüğe kaydeder</span><span class="sxs-lookup"><span data-stu-id="2030d-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="2030d-158">HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="2030d-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="2030d-159">`AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2030d-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="2030d-160">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="2030d-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="2030d-161">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="2030d-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="2030d-162">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS'yi zorunlu tutmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2030d-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="2030d-163">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri donatmak veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2030d-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="2030d-164">Genel öznitelik uygulamak için aşağıdaki kodu ekleyin. `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2030d-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="2030d-165">Tüm istekleri kullanmak önceki vurgulanan kod gerektirir `HTTPS`; bu nedenle, HTTP isteklerini dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="2030d-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="2030d-166">Aşağıdaki vurgulanmış kodu tüm HTTP isteklerini HTTPS için yeniden yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="2030d-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="2030d-167">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="2030d-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="2030d-168">Ara yazılım Ayrıca, uygulama yeniden yönlendirme yürütüldüğünde, durum kodu veya durum kodunu ve bağlantı noktası ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="2030d-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="2030d-169">Genel olarak HTTPS'yi zorunlu (`options.Filters.Add(new RequireHttpsAttribute());`) güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="2030d-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="2030d-170">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfaları için öznitelik değil olarak kabul genel HTTPS'yi zorunlu kadar güvenli.</span><span class="sxs-lookup"><span data-stu-id="2030d-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="2030d-171">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfaları eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2030d-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="2030d-172">HTTP taşıma katı güvenlik protokolü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="2030d-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="2030d-173">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="2030d-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="2030d-174">Olduğunda bir [HSTS destekleyen bir tarayıcı](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) bu üstbilgiyi alır:</span><span class="sxs-lookup"><span data-stu-id="2030d-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="2030d-175">Tarayıcıya HTTP üzerinden herhangi bir iletişim göndermeyi önler etki alanı için yapılandırma depolar.</span><span class="sxs-lookup"><span data-stu-id="2030d-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="2030d-176">Tarayıcı tüm iletişim HTTPS üzerinden zorlar.</span><span class="sxs-lookup"><span data-stu-id="2030d-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="2030d-177">Tarayıcı kullanıcı, güvenilmeyen ya da geçersiz bir sertifika kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="2030d-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="2030d-178">Tarayıcı geçici olarak bu tür bir sertifika güven etmesine istemlerini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="2030d-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="2030d-179">İstemci tarafından HSTS zorunlu kılındığından bazı sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="2030d-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="2030d-180">İstemci HSTS desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2030d-180">The client must support HSTS.</span></span>
* <span data-ttu-id="2030d-181">HSTS HSTS ilkesi oluşturmak üzere en az bir başarılı HTTPS isteğini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2030d-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="2030d-182">Uygulama gerekir her HTTP isteği denetleyin ve yeniden yönlendirme veya HTTP isteği reddedebilir.</span><span class="sxs-lookup"><span data-stu-id="2030d-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="2030d-183">ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2030d-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="2030d-184">Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="2030d-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="2030d-185">`UseHsts` HSTS ayarları yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez.</span><span class="sxs-lookup"><span data-stu-id="2030d-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="2030d-186">Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.</span><span class="sxs-lookup"><span data-stu-id="2030d-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="2030d-187">HTTPS için ilk kez uygulama, üretim ortamları için başlangıç HSTS değeri küçük bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2030d-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="2030d-188">HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2030d-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="2030d-189">HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="2030d-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="2030d-190">Aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="2030d-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="2030d-191">Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2030d-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="2030d-192">Önyükleme bölümü olmayan [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak.</span><span class="sxs-lookup"><span data-stu-id="2030d-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="2030d-193">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2030d-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="2030d-194">Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2030d-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="2030d-195">Açıkça Strıct aktarım güvenliği üst bilgi, max-age parametresini 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2030d-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="2030d-196">Aksi durumda, 30 gün için varsayılanları ayarlama.</span><span class="sxs-lookup"><span data-stu-id="2030d-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="2030d-197">Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="2030d-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="2030d-198">Ekler `example.com` dışlanacak konaklar listesine.</span><span class="sxs-lookup"><span data-stu-id="2030d-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="2030d-199">`UseHsts` şu geri döngü konakları hariç tutar:</span><span class="sxs-lookup"><span data-stu-id="2030d-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="2030d-200">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="2030d-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="2030d-201">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="2030d-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="2030d-202">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="2030d-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="2030d-203">Yukarıdaki örnekte, ek konaklarının nasıl ekleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2030d-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="2030d-204">Çevirme HTTPS projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2030d-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="2030d-205">ASP.NET Core 2.1 veya üzeri web uygulaması şablonlarıyla (Visual Studio veya dotnet komut satırı) etkinleştirme [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="2030d-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="2030d-206">HTTPS gerektirmeyen dağıtımları için HTTPS çevirme.</span><span class="sxs-lookup"><span data-stu-id="2030d-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="2030d-207">Örneğin, burada HTTPS dışarıdan ucuna her düğümde HTTPS kullanarak işlenen bazı arka uç Hizmetleri gerekli olmayan.</span><span class="sxs-lookup"><span data-stu-id="2030d-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="2030d-208">Çevirme HTTPS için:</span><span class="sxs-lookup"><span data-stu-id="2030d-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2030d-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2030d-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="2030d-210">Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="2030d-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Varlık diyagramı](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2030d-212">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2030d-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="2030d-213">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="2030d-213">Use the `--no-https` option.</span></span> <span data-ttu-id="2030d-214">Örneğin</span><span class="sxs-lookup"><span data-stu-id="2030d-214">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="2030d-215">Docker için bir geliştirici sertifikası ayarlama</span><span class="sxs-lookup"><span data-stu-id="2030d-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="2030d-216">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="2030d-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="2030d-217">Ek bilgiler</span><span class="sxs-lookup"><span data-stu-id="2030d-217">Additional information</span></span>

* [<span data-ttu-id="2030d-218">OWASP HSTS tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="2030d-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
