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
ms.openlocfilehash: b324dbcd6d28c1a8505f96da333874728e2e6a18
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="747ad-103">Bir ASP.NET Core HTTPS zorla</span><span class="sxs-lookup"><span data-stu-id="747ad-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="747ad-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="747ad-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="747ad-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="747ad-105">This document shows how to:</span></span>

- <span data-ttu-id="747ad-106">HTTPS için tüm istekleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="747ad-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="747ad-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="747ad-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="747ad-108">Yapmak **değil** kullanmak `RequireHttpsAttribute` Web API'lerde hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="747ad-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="747ad-109">`RequireHttpsAttribute` HTTP tarayıcılarından HTTPS'ye yeniden yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="747ad-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="747ad-110">API istemcileri değil anlamak veya HTTP yönlendirir HTTPS uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="747ad-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="747ad-111">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="747ad-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="747ad-112">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="747ad-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="747ad-113">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="747ad-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="747ad-114">Durum kodu 400 (Hatalı istek) ile bağlantı kapatın ve istek hizmet yok.</span><span class="sxs-lookup"><span data-stu-id="747ad-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="747ad-115">HTTPS gerektirir</span><span class="sxs-lookup"><span data-stu-id="747ad-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="747ad-116">Web uygulamaları çağrısı tüm ASP.NET Core öneririz `UseHttpsRedirection` HTTPS için tüm HTTP isteklerini yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="747ad-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="747ad-117">Varsa `UseHsts` çağrılır uygulamada, onu önce çağrılmalıdır `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="747ad-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="747ad-118">Aşağıdaki kod çağrıları `UseHttpsRedirection` içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="747ad-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="747ad-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="747ad-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="747ad-120">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="747ad-120">The following code:</span></span>

<span data-ttu-id="747ad-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="747ad-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="747ad-122">Ayarlar `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="747ad-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="747ad-123">HTTPS bağlantı noktası için 5001 ayarlar.</span><span class="sxs-lookup"><span data-stu-id="747ad-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="747ad-124">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) HTTPS gerektirecek şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="747ad-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="747ad-125">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri işaretleme veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="747ad-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="747ad-126">Öznitelik genel uygulamak için aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="747ad-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="747ad-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="747ad-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="747ad-128">Tüm istekleri kullanır önceki vurgulanmış kodu gerektirir `HTTPS`; bu nedenle, HTTP isteklerini yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="747ad-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="747ad-129">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="747ad-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="747ad-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="747ad-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="747ad-131">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="747ad-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="747ad-132">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="747ad-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="747ad-133">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfalarının özniteliğine değil olarak kabul güvenli olarak genel HTTPS gerektiren.</span><span class="sxs-lookup"><span data-stu-id="747ad-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="747ad-134">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfalarının eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="747ad-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="747ad-135">HTTP katı Aktarım güvenlik protokolünü (HSTS)</span><span class="sxs-lookup"><span data-stu-id="747ad-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="747ad-136">Başına [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP katı Aktarım güvenlik (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) özel yanıt üstbilgisi kullanımı ile bir web uygulaması tarafından belirtilen bir güvenlik katılımı yeniliktir.</span><span class="sxs-lookup"><span data-stu-id="747ad-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="747ad-137">Bu üst desteklenen bir tarayıcı aldıktan sonra bu tarayıcı iletişimlerle belirtilen etki alanı için HTTP üzerinden gönderilmesini engeller ve bunun yerine tüm iletişimler HTTPS üzerinden gönderir.</span><span class="sxs-lookup"><span data-stu-id="747ad-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="747ad-138">Ayrıca, HTTPS istekleri tarayıcılarda geçişli tıklatma önler.</span><span class="sxs-lookup"><span data-stu-id="747ad-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="747ad-139">ASP.NET 2.1 preview1 çekirdek veya sonrası ile HSTS uygulayan `UseHsts` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="747ad-139">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="747ad-140">Aşağıdaki kod çağrıları `UseHsts` zaman uygulama değil de [geliştirme modunu](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="747ad-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="747ad-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="747ad-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="747ad-142">`UseHsts` HSTS üstbilgisi tarayıcılar tarafından yüksek oranda cachable olduğundan öneri geliştirme değil.</span><span class="sxs-lookup"><span data-stu-id="747ad-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="747ad-143">Varsayılan olarak, yerel bir geri döngü adresine UseHsts dışlar.</span><span class="sxs-lookup"><span data-stu-id="747ad-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="747ad-144">Aşağıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="747ad-144">The following code:</span></span>

<span data-ttu-id="747ad-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="747ad-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="747ad-146">Strict Aktarım güvenlik üstbilgisi önyüklemesi parametresinin ayarlar.</span><span class="sxs-lookup"><span data-stu-id="747ad-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="747ad-147">Önyükleme değil parçası [RFC HSTS belirtimi](https://tools.ietf.org/html/rfc6797), yeniden yüklemeyi HSTS sitelerinde önceden yüklemek için web tarayıcısı tarafından desteklenir, ancak.</span><span class="sxs-lookup"><span data-stu-id="747ad-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="747ad-148">Bkz: [ https://hstspreload.org/ ](https://hstspreload.org/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="747ad-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="747ad-149">Etkinleştirir [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), ana bilgisayar alt etki alanları için HSTS ilkesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="747ad-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="747ad-150">Açıkça katı taşıma güvenliği üstbilgiye max-age parametresinin 60 gün olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="747ad-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="747ad-151">Aksi durumda ayarlanırsa, varsayılan olarak 30 gün.</span><span class="sxs-lookup"><span data-stu-id="747ad-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="747ad-152">Bkz: [, max-age yönergesi](https://tools.ietf.org/html/rfc6797#section-6.1.1) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="747ad-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="747ad-153">Ekler `example.com` dışlamak için ana bilgisayarlar listesine.</span><span class="sxs-lookup"><span data-stu-id="747ad-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="747ad-154">`UseHsts` şu geri döngü konakları dışlar:</span><span class="sxs-lookup"><span data-stu-id="747ad-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="747ad-155">`localhost` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="747ad-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="747ad-156">`127.0.0.1` : IPv4 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="747ad-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="747ad-157">`[::1]` : IPv6 geri döngü adresi.</span><span class="sxs-lookup"><span data-stu-id="747ad-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="747ad-158">Önceki örnekte, ek ana bilgisayar eklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="747ad-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="747ad-159">Üyelikten çıkmak HTTPS projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="747ad-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="747ad-160">Sonraki web uygulama şablonlardan (Visual Studio veya dotnet komut satırı) ve ASP.NET Core 2.1 etkinleştirmek [HTTPS yeniden yönlendirmesi](#require) ve [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="747ad-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="747ad-161">HTTPS gerektirmeyen dağıtımları için HTTPS çevirin.</span><span class="sxs-lookup"><span data-stu-id="747ad-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="747ad-162">Örneğin, burada HTTPS dışarıdan sınırında her düğümde HTTPS kullanarak işlenen bazı arka uç hizmetlerinin gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="747ad-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="747ad-163">HTTPS çevirin için:</span><span class="sxs-lookup"><span data-stu-id="747ad-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="747ad-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="747ad-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="747ad-165">İşaretini **HTTPS için Yapılandır** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="747ad-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Varlık diyagramı](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="747ad-167">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="747ad-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="747ad-168">Kullanım `--no-https` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="747ad-168">Use the `--no-https` option.</span></span> <span data-ttu-id="747ad-169">Örneğin</span><span class="sxs-lookup"><span data-stu-id="747ad-169">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="747ad-170">Bir geliştirici sertifikası için Docker ayarlama</span><span class="sxs-lookup"><span data-stu-id="747ad-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="747ad-171">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="747ad-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
