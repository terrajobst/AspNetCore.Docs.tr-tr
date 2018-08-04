---
title: ASP.NET core'da HTTPS'yi zorunlu kılma
author: rick-anderson
description: Web uygulaması nasıl bir ASP.NET Core HTTPS/TLS'ye gerektirecek şekilde gösterir.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: d8bf11d7d2df8d8b197f001570a8fab1f3262814
ms.sourcegitcommit: 4e34ce61e1e7f1317102b16012ce0742abf2cca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514810"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="45f46-103">ASP.NET core'da HTTPS'yi zorunlu kılma</span><span class="sxs-lookup"><span data-stu-id="45f46-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="45f46-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="45f46-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="45f46-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="45f46-105">This document shows how to:</span></span>

* <span data-ttu-id="45f46-106">Tüm istekler için HTTPS gerektirir.</span><span class="sxs-lookup"><span data-stu-id="45f46-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="45f46-107">Tüm HTTP isteklerini HTTPS'ye yönlendiriyor.</span><span class="sxs-lookup"><span data-stu-id="45f46-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="45f46-108">Yapmak **değil** kullanın [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) üzerinde Web API'leri, hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="45f46-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="45f46-109">`RequireHttpsAttribute` tarayıcılar HTTP'den HTTPS'ye yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="45f46-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="45f46-110">API istemcileri değil anlamak veya yeniden yönlendirmeleri HTTP'den HTTPS'ye uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="45f46-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="45f46-111">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="45f46-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="45f46-112">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="45f46-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="45f46-113">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="45f46-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="45f46-114">400 (Hatalı istek) durum koduyla bağlantıyı kapatın ve hizmet isteği yok.</span><span class="sxs-lookup"><span data-stu-id="45f46-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="45f46-115">HTTPS'yi zorunlu</span><span class="sxs-lookup"><span data-stu-id="45f46-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="45f46-116">Tüm ASP.NET Core web uygulamaları çağrı HTTPS yeniden yönlendirmesi ara yazılım öneririz ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) tüm HTTP isteklerini HTTPS için yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="45f46-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="45f46-117">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="45f46-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="45f46-118">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="45f46-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="45f46-119">Varsayılan [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="45f46-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="45f46-120">Üretim uygulamaları çağırmalıdır [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="45f46-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="45f46-121">Varsayılan [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="45f46-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="45f46-122">Aşağıdaki kod çağrıları [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) ara yazılım seçenekleri yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="45f46-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="45f46-123">Önceki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="45f46-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="45f46-124">Kümeleri [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) için `Status307TemporaryRedirect`, varsayılan değer olan.</span><span class="sxs-lookup"><span data-stu-id="45f46-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="45f46-125">Üretim uygulamaları çağırmalıdır [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="45f46-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="45f46-126">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="45f46-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="45f46-127">443 varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="45f46-127">The default value is 443.</span></span>

<span data-ttu-id="45f46-128">Aşağıdaki mekanizmalardan bağlantı noktasını otomatik olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="45f46-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="45f46-129">Ara yazılım aracılığıyla bağlantı noktaları bulabilir [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) zaman aşağıdaki koşullar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="45f46-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="45f46-130">Kestrel'i veya HTTP.sys doğrudan HTTPS uç noktaları ile kullanılan (Ayrıca uygulamanın Visual Studio Code'nın hata ayıklayıcısı ile çalıştırma için geçerlidir).</span><span class="sxs-lookup"><span data-stu-id="45f46-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="45f46-131">Yalnızca **bir HTTPS bağlantı noktası** uygulama tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="45f46-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="45f46-132">Visual Studio kullanılır:</span><span class="sxs-lookup"><span data-stu-id="45f46-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="45f46-133">IIS Express, HTTPS etkin sahiptir.</span><span class="sxs-lookup"><span data-stu-id="45f46-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="45f46-134">*launchSettings.json* ayarlar `sslPort` IIS Express için.</span><span class="sxs-lookup"><span data-stu-id="45f46-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="45f46-135">Bir uygulama bir ters proxy (örneğin, IIS, IIS Express) çalıştırın, `IServerAddressesFeature` kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="45f46-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="45f46-136">Bağlantı noktasını el ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="45f46-136">The port must be manually configured.</span></span> <span data-ttu-id="45f46-137">Bağlantı noktası olarak değil, istekleri yeniden yönlendirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="45f46-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="45f46-138">Bağlantı noktası ayarlayarak yapılandırılabilir [https_port Web ana bilgisayar yapılandırma ayarı](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="45f46-138">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="45f46-139">**Anahtar**: https_port **türü**: *dize*
**varsayılan**: varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="45f46-139">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="45f46-140">**Kullanılarak ayarlanan**: `UseSetting` 
 **ortam değişkeni**: `<PREFIX_>HTTPS_PORT` (ön ek `ASPNETCORE_` Web ana bilgisayarı kullanırken.)</span><span class="sxs-lookup"><span data-stu-id="45f46-140">**Set using**: `UseSetting`
**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="45f46-141">Bağlantı noktası URL'si ile ayarlayarak dolaylı olarak yapılandırılabilir `ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="45f46-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="45f46-142">Ortam değişkenini sunucusunu yapılandırır ve ara yazılım HTTPS bağlantı noktası üzerinden dolaylı olarak bulur `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="45f46-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="45f46-143">Bağlantı noktası ayarlanırsa:</span><span class="sxs-lookup"><span data-stu-id="45f46-143">If no port is set:</span></span>

* <span data-ttu-id="45f46-144">İstekleri yeniden yönlendirilen değildir.</span><span class="sxs-lookup"><span data-stu-id="45f46-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="45f46-145">Ara yazılım, bir uyarı günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="45f46-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="45f46-146">HTTPS yeniden yönlendirmesi ara yazılım kullanmaya alternatif (`UseHttpsRedirection`) URL yeniden yazma ara yazılımı kullanmaktır (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="45f46-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="45f46-147">`AddRedirectToHttps` yeniden yönlendirme yürütüldüğünde de durum kodunu ve bağlantı noktası ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45f46-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="45f46-148">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="45f46-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="45f46-149">HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılımın kullanılması önerilir (`UseHttpsRedirection`) Bu konuda açıklanan.</span><span class="sxs-lookup"><span data-stu-id="45f46-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="45f46-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) HTTPS'yi zorunlu tutmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="45f46-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="45f46-151">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri donatmak veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="45f46-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="45f46-152">Genel öznitelik uygulamak için aşağıdaki kodu ekleyin. `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="45f46-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="45f46-153">Tüm istekleri kullanmak önceki vurgulanan kod gerektirir `HTTPS`; bu nedenle, HTTP isteklerini dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="45f46-153">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="45f46-154">Aşağıdaki vurgulanmış kodu tüm HTTP isteklerini HTTPS için yeniden yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="45f46-154">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="45f46-155">Daha fazla bilgi için [URL yeniden yazma ara yazılımı](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="45f46-155">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="45f46-156">Ara yazılım Ayrıca, uygulama yeniden yönlendirme yürütüldüğünde, durum kodu veya durum kodunu ve bağlantı noktası ayarlamak için verir.</span><span class="sxs-lookup"><span data-stu-id="45f46-156">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="45f46-157">Genel olarak HTTPS'yi zorunlu (`options.Filters.Add(new RequireHttpsAttribute());`) güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="45f46-157">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="45f46-158">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfaları için öznitelik değil olarak kabul genel HTTPS'yi zorunlu kadar güvenli.</span><span class="sxs-lookup"><span data-stu-id="45f46-158">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="45f46-159">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfaları eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="45f46-159">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="45f46-160">HTTP taşıma katı güvenlik protokolü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="45f46-160">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="45f46-161">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı taşıma güvenliği (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) özel yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir.</span><span class="sxs-lookup"><span data-stu-id="45f46-161">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a special response header.</span></span> <span data-ttu-id="45f46-162">Bu üst bilginin HSTS destekleyen bir tarayıcı aldığında, HTTP üzerinden herhangi bir iletişim göndermeyi önler ve bunun yerine tüm iletişim HTTPS üzerinden zorlar etki alanı için yapılandırma depolar.</span><span class="sxs-lookup"><span data-stu-id="45f46-162">When a browser that supports HSTS receives this header, it stores configuration for the domain that prevents sending any communication over HTTP and instead forces all communication over HTTPS.</span></span> <span data-ttu-id="45f46-163">Ayrıca, kullanıcının'in geçici olarak bu tür bir sertifika güven etmesine tarayıcı istemlerini devre dışı bırakma, güvenilmeyen ya da geçersiz sertifikaları kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="45f46-163">It also prevents the user from using untrusted or invalid certificates, disabling the browser prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="45f46-164">ASP.NET Core 2.1 veya üzeri ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="45f46-164">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="45f46-165">Aşağıdaki kod çağrıları `UseHsts` uygulama değil ne zaman [geliştirme modu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="45f46-165">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="45f46-166">`UseHsts` HSTS üstbilgi yüksek oranda önbelleğe alınabilir olduğundan geliştirme tarayıcılar tarafından önerilmez.</span><span class="sxs-lookup"><span data-stu-id="45f46-166">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="45f46-167">Varsayılan olarak, `UseHsts` yerel geri döngü adresine dışlar.</span><span class="sxs-lookup"><span data-stu-id="45f46-167">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="45f46-168">HTTPS için ilk kez uygulama, üretim ortamları için başlangıç HSTS değeri küçük bir değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="45f46-168">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="45f46-169">HTTP HTTPS altyapısında geri gerektiği durumlarda değeri Hayır birden fazla tek günlük bir saat olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="45f46-169">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="45f46-170">HTTPS yapılandırmasının sürdürülebilirlik içinde başarılara sonra HSTS max-age değerini artırın; bir yıl buna yaygın olarak kullanılan bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="45f46-170">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="45f46-171">Aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="45f46-171">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="45f46-172">Önyük parametresi Strıct aktarım güvenliği üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="45f46-172">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="45f46-173">Preload değil parçası [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeni bir yükleme HSTS sitelerinde önceden yüklemek için web tarayıcıları tarafından desteklenir ancak.</span><span class="sxs-lookup"><span data-stu-id="45f46-173">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="45f46-174">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="45f46-174">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="45f46-175">Sağlar [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilke uygulanır.</span><span class="sxs-lookup"><span data-stu-id="45f46-175">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="45f46-176">Açıkça Strıct aktarım güvenliği üst max-age parametresini 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="45f46-176">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="45f46-177">Aksi durumda, 30 gün için varsayılanları ayarlama.</span><span class="sxs-lookup"><span data-stu-id="45f46-177">If not set, defaults to 30 days.</span></span> <span data-ttu-id="45f46-178">Bkz: [max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="45f46-178">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="45f46-179">Ekler `example.com` dışlanacak konaklar listesine.</span><span class="sxs-lookup"><span data-stu-id="45f46-179">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="45f46-180">`UseHsts` şu geri döngü konakları hariç tutar:</span><span class="sxs-lookup"><span data-stu-id="45f46-180">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="45f46-181">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="45f46-181">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="45f46-182">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="45f46-182">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="45f46-183">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="45f46-183">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="45f46-184">Yukarıdaki örnekte, ek konaklarının nasıl ekleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="45f46-184">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="45f46-185">Çevirme HTTPS projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="45f46-185">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="45f46-186">ASP.NET Core 2.1 veya üzeri web uygulaması şablonlarıyla (Visual Studio veya dotnet komut satırı) etkinleştirme [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="45f46-186">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="45f46-187">HTTPS gerektirmeyen dağıtımları için HTTPS çevirme.</span><span class="sxs-lookup"><span data-stu-id="45f46-187">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="45f46-188">Örneğin, burada HTTPS dışarıdan ucuna her düğümde HTTPS kullanarak işlenen bazı arka uç Hizmetleri gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="45f46-188">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="45f46-189">Çevirme HTTPS için:</span><span class="sxs-lookup"><span data-stu-id="45f46-189">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="45f46-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45f46-190">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="45f46-191">Onay kutusunu temizleyin **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="45f46-191">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Varlık diyagramı](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="45f46-193">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="45f46-193">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="45f46-194">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="45f46-194">Use the `--no-https` option.</span></span> <span data-ttu-id="45f46-195">Örneğin</span><span class="sxs-lookup"><span data-stu-id="45f46-195">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="45f46-196">Docker için bir geliştirici sertifikası ayarlama</span><span class="sxs-lookup"><span data-stu-id="45f46-196">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="45f46-197">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="45f46-197">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
