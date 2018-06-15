---
title: ASP.NET Core HTTPS zorla
author: rick-anderson
description: Bir ASP.NET Core HTTPS/TLS gerektiren web uygulaması gösterilmektedir.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 48a25b7ba7affe84cfa6fe16096409239c510221
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652194"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="acf40-103">ASP.NET Core HTTPS zorla</span><span class="sxs-lookup"><span data-stu-id="acf40-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="acf40-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="acf40-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="acf40-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="acf40-105">This document shows how to:</span></span>

* <span data-ttu-id="acf40-106">HTTPS için tüm istekleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="acf40-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="acf40-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="acf40-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="acf40-108">Yapmak **değil** kullanmak [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) Web API'lerde hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="acf40-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="acf40-109">`RequireHttpsAttribute` HTTP tarayıcılarından HTTPS'ye yeniden yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="acf40-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="acf40-110">API istemcileri değil anlamak veya HTTP yönlendirir HTTPS uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="acf40-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="acf40-111">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="acf40-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="acf40-112">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="acf40-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="acf40-113">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="acf40-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="acf40-114">Durum kodu 400 (Hatalı istek) ile bağlantı kapatın ve istek hizmet yok.</span><span class="sxs-lookup"><span data-stu-id="acf40-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="acf40-115">HTTPS gerektirir</span><span class="sxs-lookup"><span data-stu-id="acf40-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="acf40-116">Tüm ASP.NET Core web uygulamaları çağrı HTTPS yeniden yönlendirmesi Ara öneririz ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) için HTTPS tüm HTTP isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="acf40-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="acf40-117">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="acf40-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="acf40-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="acf40-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="acf40-119">Aşağıdaki kod çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="acf40-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="acf40-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="acf40-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="acf40-121">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="acf40-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="acf40-122">Ayarlar [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) için `Status307TemporaryRedirect`, varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="acf40-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="acf40-123">Üretim uygulamaları çağrısı [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="acf40-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="acf40-124">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="acf40-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="acf40-125">Varsayılan değer 443'tür.</span><span class="sxs-lookup"><span data-stu-id="acf40-125">The default value is 443.</span></span>

<span data-ttu-id="acf40-126">Aşağıdaki mekanizmalardan bağlantı noktasını otomatik olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="acf40-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="acf40-127">Ara yazılım aracılığıyla bağlantı noktalarını bulabilmesi için [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) zaman aşağıdaki koşullar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="acf40-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="acf40-128">Kestrel veya HTTP.sys doğrudan HTTPS uç noktalarla birlikte kullanılan (uygulama Visual Studio kodun hata ayıklayıcısı ile çalıştırma için de geçerlidir).</span><span class="sxs-lookup"><span data-stu-id="acf40-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="acf40-129">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="acf40-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="acf40-130">Visual Studio kullanılır:</span><span class="sxs-lookup"><span data-stu-id="acf40-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="acf40-131">IIS Express HTTPS etkinleştirilmiş sahiptir.</span><span class="sxs-lookup"><span data-stu-id="acf40-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="acf40-132">*launchSettings.json* ayarlar `sslPort` IIS Express için.</span><span class="sxs-lookup"><span data-stu-id="acf40-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="acf40-133">Ters Ara sunucu (örneğin, IIS, IIS Express), bir uygulama çalıştırıldığında `IServerAddressesFeature` kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="acf40-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="acf40-134">Bağlantı noktasını el ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="acf40-134">The port must be manually configured.</span></span> <span data-ttu-id="acf40-135">Bağlantı noktası ayarlanmamışsa, istekleri yeniden yönlendirilen değil.</span><span class="sxs-lookup"><span data-stu-id="acf40-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="acf40-136">Bağlantı noktası ayarlanarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="acf40-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="acf40-137">`ASPNETCORE_HTTPS_PORT` Ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="acf40-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="acf40-138">`http_port` ana bilgisayar yapılandırma anahtarı (örneğin, aracılığıyla *hostsettings.json* veya komut satırı bağımsız değişkeni).</span><span class="sxs-lookup"><span data-stu-id="acf40-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="acf40-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="acf40-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="acf40-140">Bağlantı noktası için 5001 ayarlanması gösterilmektedir önceki örneğe bakın.</span><span class="sxs-lookup"><span data-stu-id="acf40-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="acf40-141">Bağlantı noktası URL'si ile ayarlayarak dolaylı olarak yapılandırılabilir `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="acf40-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="acf40-142">Ortam değişkeni sunucusu yapılandırır ve ara yazılım HTTPS bağlantı noktası aracılığıyla dolaylı olarak bulur `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="acf40-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="acf40-143">Bağlantı noktası yok ayarlanırsa:</span><span class="sxs-lookup"><span data-stu-id="acf40-143">If no port is set:</span></span>

* <span data-ttu-id="acf40-144">İstekleri yeniden yönlendirilen değil.</span><span class="sxs-lookup"><span data-stu-id="acf40-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="acf40-145">Ara yazılım bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="acf40-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="acf40-146">HTTPS yeniden yönlendirmesi Ara kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma işlemi Ara kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="acf40-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="acf40-147">`AddRedirectToHttps` yeniden yönlendirme çalıştırıldığında de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="acf40-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="acf40-148">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="acf40-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="acf40-149">HTTPS için ek yönlendirme kuralları gereksinimi olmadan yönlendirirken HTTPS yeniden yönlendirmesi ara yazılımı kullanmanız önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="acf40-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="acf40-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS gerektirecek şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="acf40-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="acf40-151">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri işaretleme veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="acf40-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="acf40-152">Öznitelik genel uygulamak için aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="acf40-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="acf40-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="acf40-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="acf40-154">Tüm istekleri kullanır önceki vurgulanmış kodu gerektirir `HTTPS`; bu nedenle, HTTP isteklerini yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="acf40-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="acf40-155">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="acf40-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="acf40-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="acf40-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="acf40-157">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="acf40-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="acf40-158">Ara yazılım de yeniden yönlendirme çalıştırıldığında durum kodunu veya durum kodu ve bağlantı noktası ayarlamak için uygulama izin verir.</span><span class="sxs-lookup"><span data-stu-id="acf40-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="acf40-159">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="acf40-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="acf40-160">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfalarının özniteliğine değil olarak kabul güvenli olarak genel HTTPS gerektiren.</span><span class="sxs-lookup"><span data-stu-id="acf40-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="acf40-161">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfalarının eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="acf40-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="acf40-162">HTTP katı Aktarım güvenlik protokolünü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="acf40-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="acf40-163">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı Aktarım güvenlik (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) özel yanıt üstbilgisi kullanımı ile bir web uygulaması tarafından belirtilen bir güvenlik katılımı yeniliktir.</span><span class="sxs-lookup"><span data-stu-id="acf40-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="acf40-164">Bu üst desteklenen bir tarayıcı aldıktan sonra bu tarayıcı iletişimlerle belirtilen etki alanı için HTTP üzerinden gönderilmesini engeller ve bunun yerine tüm iletişimler HTTPS üzerinden gönderir.</span><span class="sxs-lookup"><span data-stu-id="acf40-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="acf40-165">Ayrıca, HTTPS istekleri tarayıcılarda geçişli tıklatma önler.</span><span class="sxs-lookup"><span data-stu-id="acf40-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="acf40-166">ASP.NET Core 2.1 veya sonrası ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="acf40-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="acf40-167">Aşağıdaki kod çağrıları `UseHsts` zaman uygulama değil de [geliştirme modunu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="acf40-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="acf40-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="acf40-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="acf40-169">`UseHsts` HSTS üstbilgisi tarayıcılar tarafından yüksek oranda cachable olduğundan öneri geliştirme değil.</span><span class="sxs-lookup"><span data-stu-id="acf40-169">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="acf40-170">Varsayılan olarak, yerel bir geri döngü adresine UseHsts dışlar.</span><span class="sxs-lookup"><span data-stu-id="acf40-170">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="acf40-171">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="acf40-171">The following code:</span></span>

<span data-ttu-id="acf40-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="acf40-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="acf40-173">Strict Aktarım güvenlik üstbilgisi önyüklemesi parametresinin ayarlar.</span><span class="sxs-lookup"><span data-stu-id="acf40-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="acf40-174">Önyükleme değil parçası [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeniden yüklemeyi HSTS sitelerinde önceden yüklemek için web tarayıcısı tarafından desteklenir, ancak.</span><span class="sxs-lookup"><span data-stu-id="acf40-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="acf40-175">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="acf40-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="acf40-176">Etkinleştirir [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilkesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="acf40-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="acf40-177">Açıkça katı taşıma güvenliği üstbilgiye max-age parametresinin 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="acf40-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="acf40-178">Aksi durumda ayarlanırsa, varsayılan olarak 30 gün.</span><span class="sxs-lookup"><span data-stu-id="acf40-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="acf40-179">Bkz: [, max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="acf40-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="acf40-180">Ekler `example.com` dışlamak için ana bilgisayarlar listesine.</span><span class="sxs-lookup"><span data-stu-id="acf40-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="acf40-181">`UseHsts` şu geri döngü konakları dışlar:</span><span class="sxs-lookup"><span data-stu-id="acf40-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="acf40-182">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="acf40-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="acf40-183">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="acf40-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="acf40-184">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="acf40-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="acf40-185">Önceki örnekte, ek ana bilgisayar eklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="acf40-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="acf40-186">Üyelikten çıkmak HTTPS projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="acf40-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="acf40-187">ASP.NET Core 2.1 veya üzeri bir web uygulaması şablonlardan (Visual Studio veya dotnet komut satırı) etkinleştirme [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="acf40-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="acf40-188">HTTPS gerektirmeyen dağıtımları için HTTPS çevirin.</span><span class="sxs-lookup"><span data-stu-id="acf40-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="acf40-189">Örneğin, burada HTTPS dışarıdan sınırında her düğümde HTTPS kullanarak işlenen bazı arka uç hizmetlerinin gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="acf40-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="acf40-190">HTTPS çevirin için:</span><span class="sxs-lookup"><span data-stu-id="acf40-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="acf40-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acf40-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="acf40-192">İşaretini **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="acf40-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Varlık diyagramı](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="acf40-194">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="acf40-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="acf40-195">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="acf40-195">Use the `--no-https` option.</span></span> <span data-ttu-id="acf40-196">Örneğin</span><span class="sxs-lookup"><span data-stu-id="acf40-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="acf40-198">Bir geliştirici sertifikası için Docker ayarlama</span><span class="sxs-lookup"><span data-stu-id="acf40-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="acf40-199">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="acf40-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
