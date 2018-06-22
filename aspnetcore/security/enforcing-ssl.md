---
title: ASP.NET Core HTTPS zorla
author: rick-anderson
description: Bir ASP.NET Core HTTPS/TLS gerektiren web uygulaması gösterilmektedir.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6a16bb2253fcb6e81a294f1c484db1a3e80796e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277276"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="259fc-103">ASP.NET Core HTTPS zorla</span><span class="sxs-lookup"><span data-stu-id="259fc-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="259fc-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="259fc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="259fc-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="259fc-105">This document shows how to:</span></span>

* <span data-ttu-id="259fc-106">HTTPS için tüm istekleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="259fc-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="259fc-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="259fc-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="259fc-108">Yapmak **değil** kullanmak [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) Web API'lerde hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="259fc-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="259fc-109">`RequireHttpsAttribute` HTTP tarayıcılarından HTTPS'ye yeniden yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="259fc-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="259fc-110">API istemcileri değil anlamak veya HTTP yönlendirir HTTPS uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="259fc-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="259fc-111">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="259fc-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="259fc-112">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="259fc-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="259fc-113">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="259fc-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="259fc-114">Durum kodu 400 (Hatalı istek) ile bağlantı kapatın ve istek hizmet yok.</span><span class="sxs-lookup"><span data-stu-id="259fc-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="259fc-115">HTTPS gerektirir</span><span class="sxs-lookup"><span data-stu-id="259fc-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="259fc-116">Tüm ASP.NET Core web uygulamaları çağrı HTTPS yeniden yönlendirmesi Ara öneririz ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) için HTTPS tüm HTTP isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="259fc-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="259fc-117">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="259fc-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="259fc-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="259fc-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="259fc-119">Aşağıdaki kod çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="259fc-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="259fc-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="259fc-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="259fc-121">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="259fc-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="259fc-122">Ayarlar [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) için `Status307TemporaryRedirect`, varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="259fc-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="259fc-123">Üretim uygulamaları çağrısı [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="259fc-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="259fc-124">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="259fc-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="259fc-125">Varsayılan değer 443'tür.</span><span class="sxs-lookup"><span data-stu-id="259fc-125">The default value is 443.</span></span>

<span data-ttu-id="259fc-126">Aşağıdaki mekanizmalardan bağlantı noktasını otomatik olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="259fc-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="259fc-127">Ara yazılım aracılığıyla bağlantı noktalarını bulabilmesi için [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) zaman aşağıdaki koşullar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="259fc-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="259fc-128">Kestrel veya HTTP.sys doğrudan HTTPS uç noktalarla birlikte kullanılan (uygulama Visual Studio kodun hata ayıklayıcısı ile çalıştırma için de geçerlidir).</span><span class="sxs-lookup"><span data-stu-id="259fc-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="259fc-129">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="259fc-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="259fc-130">Visual Studio kullanılır:</span><span class="sxs-lookup"><span data-stu-id="259fc-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="259fc-131">IIS Express HTTPS etkinleştirilmiş sahiptir.</span><span class="sxs-lookup"><span data-stu-id="259fc-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="259fc-132">*launchSettings.json* ayarlar `sslPort` IIS Express için.</span><span class="sxs-lookup"><span data-stu-id="259fc-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="259fc-133">Ters Ara sunucu (örneğin, IIS, IIS Express), bir uygulama çalıştırıldığında `IServerAddressesFeature` kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="259fc-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="259fc-134">Bağlantı noktasını el ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="259fc-134">The port must be manually configured.</span></span> <span data-ttu-id="259fc-135">Bağlantı noktası ayarlanmamışsa, istekleri yeniden yönlendirilen değil.</span><span class="sxs-lookup"><span data-stu-id="259fc-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="259fc-136">Bağlantı noktası ayarlanarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="259fc-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="259fc-137">`ASPNETCORE_HTTPS_PORT` Ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="259fc-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="259fc-138">`http_port` ana bilgisayar yapılandırma anahtarı (örneğin, aracılığıyla *hostsettings.json* veya komut satırı bağımsız değişkeni).</span><span class="sxs-lookup"><span data-stu-id="259fc-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="259fc-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="259fc-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="259fc-140">Bağlantı noktası için 5001 ayarlanması gösterilmektedir önceki örneğe bakın.</span><span class="sxs-lookup"><span data-stu-id="259fc-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="259fc-141">Bağlantı noktası URL'si ile ayarlayarak dolaylı olarak yapılandırılabilir `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="259fc-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="259fc-142">Ortam değişkeni sunucusu yapılandırır ve ara yazılım HTTPS bağlantı noktası aracılığıyla dolaylı olarak bulur `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="259fc-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="259fc-143">Bağlantı noktası yok ayarlanırsa:</span><span class="sxs-lookup"><span data-stu-id="259fc-143">If no port is set:</span></span>

* <span data-ttu-id="259fc-144">İstekleri yeniden yönlendirilen değil.</span><span class="sxs-lookup"><span data-stu-id="259fc-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="259fc-145">Ara yazılım bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="259fc-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="259fc-146">HTTPS yeniden yönlendirmesi Ara kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma işlemi Ara kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="259fc-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="259fc-147">`AddRedirectToHttps` yeniden yönlendirme çalıştırıldığında de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="259fc-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="259fc-148">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="259fc-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="259fc-149">HTTPS için ek yönlendirme kuralları gereksinimi olmadan yönlendirirken HTTPS yeniden yönlendirmesi ara yazılımı kullanmanız önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="259fc-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="259fc-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS gerektirecek şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="259fc-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="259fc-151">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri işaretleme veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="259fc-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="259fc-152">Öznitelik genel uygulamak için aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="259fc-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="259fc-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="259fc-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="259fc-154">Tüm istekleri kullanır önceki vurgulanmış kodu gerektirir `HTTPS`; bu nedenle, HTTP isteklerini yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="259fc-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="259fc-155">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="259fc-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="259fc-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="259fc-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="259fc-157">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="259fc-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="259fc-158">Ara yazılım de yeniden yönlendirme çalıştırıldığında durum kodunu veya durum kodu ve bağlantı noktası ayarlamak için uygulama izin verir.</span><span class="sxs-lookup"><span data-stu-id="259fc-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="259fc-159">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="259fc-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="259fc-160">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfalarının özniteliğine değil olarak kabul güvenli olarak genel HTTPS gerektiren.</span><span class="sxs-lookup"><span data-stu-id="259fc-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="259fc-161">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfalarının eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="259fc-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="259fc-162">HTTP katı Aktarım güvenlik protokolünü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="259fc-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="259fc-163">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı Aktarım güvenlik (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) özel yanıt üstbilgisi kullanımı ile bir web uygulaması tarafından belirtilen bir güvenlik katılımı yeniliktir.</span><span class="sxs-lookup"><span data-stu-id="259fc-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="259fc-164">Bu üst desteklenen bir tarayıcı aldıktan sonra bu tarayıcı iletişimlerle belirtilen etki alanı için HTTP üzerinden gönderilmesini engeller ve bunun yerine tüm iletişimler HTTPS üzerinden gönderir.</span><span class="sxs-lookup"><span data-stu-id="259fc-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="259fc-165">Ayrıca, HTTPS istekleri tarayıcılarda geçişli tıklatma önler.</span><span class="sxs-lookup"><span data-stu-id="259fc-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="259fc-166">ASP.NET Core 2.1 veya sonrası ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="259fc-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="259fc-167">Aşağıdaki kod çağrıları `UseHsts` zaman uygulama değil de [geliştirme modunu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="259fc-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="259fc-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="259fc-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="259fc-169">`UseHsts` HSTS üstbilgisi yüksek oranda alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez.</span><span class="sxs-lookup"><span data-stu-id="259fc-169">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="259fc-170">Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.</span><span class="sxs-lookup"><span data-stu-id="259fc-170">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="259fc-171">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="259fc-171">The following code:</span></span>

<span data-ttu-id="259fc-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="259fc-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="259fc-173">Strict Aktarım güvenlik üstbilgisi önyüklemesi parametresinin ayarlar.</span><span class="sxs-lookup"><span data-stu-id="259fc-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="259fc-174">Önyükleme değil parçası [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeniden yüklemeyi HSTS sitelerinde önceden yüklemek için web tarayıcısı tarafından desteklenir, ancak.</span><span class="sxs-lookup"><span data-stu-id="259fc-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="259fc-175">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="259fc-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="259fc-176">Etkinleştirir [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilkesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="259fc-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="259fc-177">Açıkça katı taşıma güvenliği üstbilgiye max-age parametresinin 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="259fc-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="259fc-178">Aksi durumda ayarlanırsa, varsayılan olarak 30 gün.</span><span class="sxs-lookup"><span data-stu-id="259fc-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="259fc-179">Bkz: [, max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="259fc-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="259fc-180">Ekler `example.com` dışlamak için ana bilgisayarlar listesine.</span><span class="sxs-lookup"><span data-stu-id="259fc-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="259fc-181">`UseHsts` şu geri döngü konakları dışlar:</span><span class="sxs-lookup"><span data-stu-id="259fc-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="259fc-182">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="259fc-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="259fc-183">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="259fc-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="259fc-184">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="259fc-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="259fc-185">Önceki örnekte, ek ana bilgisayar eklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="259fc-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="259fc-186">Üyelikten çıkmak HTTPS projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="259fc-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="259fc-187">ASP.NET Core 2.1 veya üzeri bir web uygulaması şablonlardan (Visual Studio veya dotnet komut satırı) etkinleştirme [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="259fc-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="259fc-188">HTTPS gerektirmeyen dağıtımları için HTTPS çevirin.</span><span class="sxs-lookup"><span data-stu-id="259fc-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="259fc-189">Örneğin, burada HTTPS dışarıdan sınırında her düğümde HTTPS kullanarak işlenen bazı arka uç hizmetlerinin gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="259fc-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="259fc-190">HTTPS çevirin için:</span><span class="sxs-lookup"><span data-stu-id="259fc-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="259fc-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="259fc-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="259fc-192">İşaretini **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="259fc-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Varlık diyagramı](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="259fc-194">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="259fc-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="259fc-195">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="259fc-195">Use the `--no-https` option.</span></span> <span data-ttu-id="259fc-196">Örneğin</span><span class="sxs-lookup"><span data-stu-id="259fc-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="259fc-198">Bir geliştirici sertifikası için Docker ayarlama</span><span class="sxs-lookup"><span data-stu-id="259fc-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="259fc-199">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="259fc-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
