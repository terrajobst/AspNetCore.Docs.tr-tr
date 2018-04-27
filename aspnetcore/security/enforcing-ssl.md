---
title: Bir ASP.NET Core HTTPS zorla
author: rick-anderson
description: Bir ASP.NET Core HTTPS/TLS gerektiren web uygulaması gösterilmektedir.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0509bebe430c6ba213031a2cb7cb91bb7a39566d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="a677b-103">Bir ASP.NET Core HTTPS zorla</span><span class="sxs-lookup"><span data-stu-id="a677b-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="a677b-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a677b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a677b-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="a677b-105">This document shows how to:</span></span>

- <span data-ttu-id="a677b-106">HTTPS için tüm istekleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a677b-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="a677b-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a677b-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="a677b-108">Yapmak **değil** kullanmak `RequireHttpsAttribute` Web API'lerde hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="a677b-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a677b-109">`RequireHttpsAttribute` HTTP tarayıcılarından HTTPS'ye yeniden yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="a677b-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a677b-110">API istemcileri değil anlamak veya HTTP yönlendirir HTTPS uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="a677b-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a677b-111">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="a677b-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a677b-112">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="a677b-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="a677b-113">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="a677b-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="a677b-114">Durum kodu 400 (Hatalı istek) ile bağlantı kapatın ve istek hizmet yok.</span><span class="sxs-lookup"><span data-stu-id="a677b-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="a677b-115">HTTPS gerektirir</span><span class="sxs-lookup"><span data-stu-id="a677b-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="a677b-117">Web uygulamaları çağrısı tüm ASP.NET Core öneririz `UseHttpsRedirection` HTTPS için tüm HTTP isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="a677b-117">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="a677b-118">Varsa `UseHsts` çağrılır uygulamada, onu önce çağrılmalıdır `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="a677b-118">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="a677b-119">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a677b-119">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="a677b-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="a677b-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="a677b-121">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="a677b-121">The following code:</span></span>

<span data-ttu-id="a677b-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="a677b-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="a677b-123">Ayarlar `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="a677b-123">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="a677b-124">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a677b-124">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a677b-125">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) HTTPS gerektirecek şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a677b-125">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="a677b-126">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri işaretleme veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="a677b-126">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a677b-127">Öznitelik genel uygulamak için aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a677b-127">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="a677b-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="a677b-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="a677b-129">Tüm istekleri kullanır önceki vurgulanmış kodu gerektirir `HTTPS`; bu nedenle, HTTP isteklerini yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="a677b-129">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a677b-130">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="a677b-130">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="a677b-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="a677b-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="a677b-132">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a677b-132">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="a677b-133">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="a677b-133">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a677b-134">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfalarının özniteliğine değil olarak kabul güvenli olarak genel HTTPS gerektiren.</span><span class="sxs-lookup"><span data-stu-id="a677b-134">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a677b-135">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfalarının eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a677b-135">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="a677b-136">HTTP katı Aktarım güvenlik protokolünü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a677b-136">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="a677b-137">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı Aktarım güvenlik (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) özel yanıt üstbilgisi kullanımı ile bir web uygulaması tarafından belirtilen bir güvenlik katılımı yeniliktir.</span><span class="sxs-lookup"><span data-stu-id="a677b-137">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="a677b-138">Bu üst desteklenen bir tarayıcı aldıktan sonra bu tarayıcı iletişimlerle belirtilen etki alanı için HTTP üzerinden gönderilmesini engeller ve bunun yerine tüm iletişimler HTTPS üzerinden gönderir.</span><span class="sxs-lookup"><span data-stu-id="a677b-138">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="a677b-139">Ayrıca, HTTPS istekleri tarayıcılarda geçişli tıklatma önler.</span><span class="sxs-lookup"><span data-stu-id="a677b-139">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="a677b-140">ASP.NET 2.1 preview1 çekirdek veya sonrası ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a677b-140">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="a677b-141">Aşağıdaki kod çağrıları `UseHsts` zaman uygulama değil de [geliştirme modunu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="a677b-141">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="a677b-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="a677b-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="a677b-143">`UseHsts` HSTS üstbilgisi tarayıcılar tarafından yüksek oranda cachable olduğundan öneri geliştirme değil.</span><span class="sxs-lookup"><span data-stu-id="a677b-143">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="a677b-144">Varsayılan olarak, yerel bir geri döngü adresine UseHsts dışlar.</span><span class="sxs-lookup"><span data-stu-id="a677b-144">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="a677b-145">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="a677b-145">The following code:</span></span>

<span data-ttu-id="a677b-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="a677b-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="a677b-147">Strict Aktarım güvenlik üstbilgisi önyüklemesi parametresinin ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a677b-147">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="a677b-148">Önyükleme değil parçası [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeniden yüklemeyi HSTS sitelerinde önceden yüklemek için web tarayıcısı tarafından desteklenir, ancak.</span><span class="sxs-lookup"><span data-stu-id="a677b-148">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="a677b-149">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a677b-149">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="a677b-150">Etkinleştirir [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilkesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a677b-150">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="a677b-151">Açıkça katı taşıma güvenliği üstbilgiye max-age parametresinin 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a677b-151">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="a677b-152">Aksi durumda ayarlanırsa, varsayılan olarak 30 gün.</span><span class="sxs-lookup"><span data-stu-id="a677b-152">If not set, defaults to 30 days.</span></span> <span data-ttu-id="a677b-153">Bkz: [, max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a677b-153">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="a677b-154">Ekler `example.com` dışlamak için ana bilgisayarlar listesine.</span><span class="sxs-lookup"><span data-stu-id="a677b-154">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="a677b-155">`UseHsts` şu geri döngü konakları dışlar:</span><span class="sxs-lookup"><span data-stu-id="a677b-155">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="a677b-156">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="a677b-156">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a677b-157">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="a677b-157">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a677b-158">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="a677b-158">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="a677b-159">Önceki örnekte, ek ana bilgisayar eklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a677b-159">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="a677b-160">Üyelikten çıkmak HTTPS projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a677b-160">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="a677b-161">Sonraki web uygulama şablonlardan (Visual Studio veya dotnet komut satırı) ve ASP.NET Core 2.1 etkinleştirmek [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="a677b-161">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="a677b-162">HTTPS gerektirmeyen dağıtımları için HTTPS çevirin.</span><span class="sxs-lookup"><span data-stu-id="a677b-162">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="a677b-163">Örneğin, burada HTTPS dışarıdan sınırında her düğümde HTTPS kullanarak işlenen bazı arka uç hizmetlerinin gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a677b-163">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="a677b-164">HTTPS çevirin için:</span><span class="sxs-lookup"><span data-stu-id="a677b-164">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a677b-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a677b-165">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="a677b-166">İşaretini **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="a677b-166">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Varlık diyagramı](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a677b-168">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a677b-168">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="a677b-169">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="a677b-169">Use the `--no-https` option.</span></span> <span data-ttu-id="a677b-170">Örneğin</span><span class="sxs-lookup"><span data-stu-id="a677b-170">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end