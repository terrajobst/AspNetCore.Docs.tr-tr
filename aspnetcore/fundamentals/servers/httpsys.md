---
title: ASP.NET Core 'de HTTP. sys Web sunucusu uygulama
author: guardrex
description: Windows üzerinde ASP.NET Core için bir Web sunucusu olan HTTP. sys hakkında bilgi edinin. HTTP. sys çekirdek modu sürücüsü üzerine inşa edilen HTTP. sys, Kestrel için IIS olmadan doğrudan Internet bağlantısı için kullanılabilen bir alternatiftir.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: b9adbdd83b3c4e1eeaadcf99fa3ee6cb41f67f8e
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310513"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="926fb-104">ASP.NET Core 'de HTTP. sys Web sunucusu uygulama</span><span class="sxs-lookup"><span data-stu-id="926fb-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="926fb-105">[Tom Dykstra](https://github.com/tdykstra), [Chris](https://github.com/Tratcher), ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="926fb-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="926fb-106">[Http. sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) , yalnızca Windows üzerinde çalışan [ASP.NET Core için bir Web sunucusudur](xref:fundamentals/servers/index) .</span><span class="sxs-lookup"><span data-stu-id="926fb-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="926fb-107">HTTP. sys, [Kestrel](xref:fundamentals/servers/kestrel) Server için bir alternatiftir ve Kestrel tarafından sağlamayan bazı özellikler sunar.</span><span class="sxs-lookup"><span data-stu-id="926fb-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="926fb-108">HTTP. sys [ASP.NET Core modülüyle](xref:host-and-deploy/aspnet-core-module) uyumlu DEĞILDIR ve ııs veya IIS Express ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="926fb-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="926fb-109">HTTP. sys aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="926fb-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="926fb-110">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="926fb-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="926fb-111">Bağlantı noktası Paylaşımı</span><span class="sxs-lookup"><span data-stu-id="926fb-111">Port sharing</span></span>
* <span data-ttu-id="926fb-112">SNı ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="926fb-112">HTTPS with SNI</span></span>
* <span data-ttu-id="926fb-113">TLS üzerinden HTTP/2 (Windows 10 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="926fb-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="926fb-114">Doğrudan dosya iletimi</span><span class="sxs-lookup"><span data-stu-id="926fb-114">Direct file transmission</span></span>
* <span data-ttu-id="926fb-115">Yanıtları Önbelleğe Alma</span><span class="sxs-lookup"><span data-stu-id="926fb-115">Response caching</span></span>
* <span data-ttu-id="926fb-116">WebSockets (Windows 8 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="926fb-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="926fb-117">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="926fb-117">Supported Windows versions:</span></span>

* <span data-ttu-id="926fb-118">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="926fb-118">Windows 7 or later</span></span>
* <span data-ttu-id="926fb-119">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="926fb-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="926fb-120">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="926fb-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="926fb-121">HTTP. sys ne zaman kullanılır</span><span class="sxs-lookup"><span data-stu-id="926fb-121">When to use HTTP.sys</span></span>

<span data-ttu-id="926fb-122">HTTP. sys, şu durumlarda olduğu dağıtımlar için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="926fb-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="926fb-123">Sunucuyu IIS kullanmadan doğrudan Internet 'e sunmaya gerek vardır.</span><span class="sxs-lookup"><span data-stu-id="926fb-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP. sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="926fb-125">İç dağıtım, [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)gibi Kestrel 'de kullanılamayan bir özelliği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="926fb-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP. sys doğrudan iç ağla iletişim kurar](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="926fb-127">HTTP. sys pek çok tür saldırılara karşı koruyan ve tam özellikli bir Web sunucusu için sağlamlık, güvenlik ve ölçeklenebilirlik sağlayan çok sayıda teknolojiden oluşur.</span><span class="sxs-lookup"><span data-stu-id="926fb-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="926fb-128">IIS, HTTP. sys ' nin üstünde HTTP dinleyicisi olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="926fb-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="926fb-129">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="926fb-129">HTTP/2 support</span></span>

<span data-ttu-id="926fb-130">Aşağıdaki temel gereksinimler karşılanıyorsa ASP.NET Core uygulamalar için [http/2](https://httpwg.org/specs/rfc7540.html) etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="926fb-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="926fb-131">Windows Server 2016/Windows 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="926fb-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="926fb-132">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="926fb-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="926fb-133">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="926fb-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="926fb-134">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="926fb-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="926fb-135">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="926fb-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="926fb-136">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="926fb-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="926fb-137">HTTP/2 bağlantısı kurulmadıysa, bağlantı HTTP/1.1 'ye geri döner.</span><span class="sxs-lookup"><span data-stu-id="926fb-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="926fb-138">Windows 'un gelecek bir sürümünde http/2 yapılandırma bayrakları http/2 ' yi HTTP. sys ile devre dışı bırakma özelliği de dahil olmak üzere kullanılabilir olacaktır.</span><span class="sxs-lookup"><span data-stu-id="926fb-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="926fb-139">Kerberos ile çekirdek modu kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="926fb-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="926fb-140">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder.</span><span class="sxs-lookup"><span data-stu-id="926fb-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="926fb-141">Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="926fb-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="926fb-142">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="926fb-143">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="926fb-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="926fb-144">HTTP. sys kullanma</span><span class="sxs-lookup"><span data-stu-id="926fb-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="926fb-145">ASP.NET Core uygulamasını HTTP. sys kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="926fb-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="926fb-146">[Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) ([NuGet.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) kullanılırken proje dosyasındaki bir paket başvurusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="926fb-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)).</span></span> <span data-ttu-id="926fb-147">`Microsoft.AspNetCore.App` Metapackage 'i kullanmıyorsanız [Microsoft. aspnetcore. Server. httpsys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="926fb-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

::: moniker-end

<span data-ttu-id="926fb-148">Ana bilgisayarı oluştururken, gerekli <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>herhangi bir yöntemi belirterek uzantıyönteminiçağırın.<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*></span><span class="sxs-lookup"><span data-stu-id="926fb-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building the host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>.</span></span> <span data-ttu-id="926fb-149">Aşağıdaki örnek, seçeneklerini varsayılan değerlerine ayarlar:</span><span class="sxs-lookup"><span data-stu-id="926fb-149">The following example sets options to their default values:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=4-12)]

::: moniker-end

<span data-ttu-id="926fb-150">Ek HTTP. sys yapılandırması, [kayıt defteri ayarları](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)aracılığıyla işlenir.</span><span class="sxs-lookup"><span data-stu-id="926fb-150">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="926fb-151">**HTTP. sys seçenekleri**</span><span class="sxs-lookup"><span data-stu-id="926fb-151">**HTTP.sys options**</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="926fb-152">Özellik</span><span class="sxs-lookup"><span data-stu-id="926fb-152">Property</span></span> | <span data-ttu-id="926fb-153">Açıklama</span><span class="sxs-lookup"><span data-stu-id="926fb-153">Description</span></span> | <span data-ttu-id="926fb-154">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="926fb-154">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="926fb-155">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="926fb-155">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="926fb-156">`HttpContext.Request.Body` Ve`HttpContext.Response.Body`için zaman uyumlu giriş/çıkışa izin verilip verilmeyeceğini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="926fb-156">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `false` |
| [<span data-ttu-id="926fb-157">Authentication. AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="926fb-157">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="926fb-158">Anonim isteklere izin verin.</span><span class="sxs-lookup"><span data-stu-id="926fb-158">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="926fb-159">Authentication. düzenleri</span><span class="sxs-lookup"><span data-stu-id="926fb-159">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="926fb-160">İzin verilen kimlik doğrulama düzenlerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="926fb-160">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="926fb-161">Dinleyici elden atılırken önce herhangi bir zamanda değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-161">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="926fb-162">Değerler [authenticationdüzenlerinin numaralandırması](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes)tarafından sağlanır: `Basic`, `Negotiate` `Kerberos`, `None`,, ve `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="926fb-162">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="926fb-163">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="926fb-163">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="926fb-164">Uygun üst bilgileri içeren yanıtlar için [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) önbelleğe almayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="926fb-164">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="926fb-165">Yanıt, `Set-Cookie` `Vary`veya üstbilgileriiçermeyebilir.`Pragma`</span><span class="sxs-lookup"><span data-stu-id="926fb-165">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="926fb-166">`Cache-Control` Bir`shared-max-age`ya dadeğeri`Expires` ya da bir üst bilgi içermelidir. `max-age` `public`</span><span class="sxs-lookup"><span data-stu-id="926fb-166">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="926fb-167">En fazla eşzamanlı kabul sayısı.</span><span class="sxs-lookup"><span data-stu-id="926fb-167">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="926fb-168">5 &times; [ortam.<br> ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="926fb-168">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="926fb-169">Kabul edilecek eşzamanlı bağlantı sayısı üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="926fb-169">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="926fb-170">Sonsuz `-1` için kullanın.</span><span class="sxs-lookup"><span data-stu-id="926fb-170">Use `-1` for infinite.</span></span> <span data-ttu-id="926fb-171">Kayıt `null` defterinin makine genelindeki ayarını kullanmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="926fb-171">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="926fb-172">sayısız</span><span class="sxs-lookup"><span data-stu-id="926fb-172">(unlimited)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="926fb-173"><a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="926fb-173">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="926fb-174">30000000 bayt</span><span class="sxs-lookup"><span data-stu-id="926fb-174">30000000 bytes</span></span><br><span data-ttu-id="926fb-175">(~ 28,6 MB)</span><span class="sxs-lookup"><span data-stu-id="926fb-175">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="926fb-176">Sıraya alınabilen en fazla istek sayısı.</span><span class="sxs-lookup"><span data-stu-id="926fb-176">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="926fb-177">1000</span><span class="sxs-lookup"><span data-stu-id="926fb-177">1000</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="926fb-178">İstemci bağlantısının kesilmesinden kaynaklanan yanıt gövdesi yazmasının, özel durumlar oluşturması veya normal şekilde tamamlanması gerektiğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="926fb-178">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="926fb-179">(normal olarak)</span><span class="sxs-lookup"><span data-stu-id="926fb-179">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="926fb-180">Kayıt defterinde da yapılandırılabilen http <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> . sys yapılandırmasını kullanıma sunun.</span><span class="sxs-lookup"><span data-stu-id="926fb-180">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="926fb-181">Varsayılan değerler de dahil olmak üzere her bir ayar hakkında daha fazla bilgi edinmek için API bağlantılarını izleyin:</span><span class="sxs-lookup"><span data-stu-id="926fb-181">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="926fb-182">HTTP sunucusu API 'sinin varlık gövdesini etkin tut bağlantısı üzerinde boşaltmasına izin verilen [TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; süresi.</span><span class="sxs-lookup"><span data-stu-id="926fb-182">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="926fb-183">[TimeoutManager. entitybody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; zamanına, istek varlığı gövdesinin gelmesi için izin verildi.</span><span class="sxs-lookup"><span data-stu-id="926fb-183">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="926fb-184">HTTP sunucusu API 'sinin istek üst bilgisini ayrıştırması için [TimeoutManager. headerwaıt](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; zamanına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-184">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="926fb-185">[TimeoutManager. boştaki bir bağlantı için ıdletimeout bağlantı](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; zamanına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-185">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="926fb-186">[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; yanıt için en düşük gönderme hızı.</span><span class="sxs-lookup"><span data-stu-id="926fb-186">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="926fb-187">[TimeoutManager.](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; isteğin, istek sırasında, uygulamanın onu çekmeden önce kalmasına izin verilen RequestQueue süresi.</span><span class="sxs-lookup"><span data-stu-id="926fb-187">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="926fb-188">HTTP. sys ile kaydolmak içinöğesinibelirtin.<xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection></span><span class="sxs-lookup"><span data-stu-id="926fb-188">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="926fb-189">En yararlı olan [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), koleksiyona bir ön ek eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926fb-189">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="926fb-190">Bunlar, dinleyici elden atılıyor öncesinde herhangi bir zamanda değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-190">These may be modified at any time prior to disposing the listener.</span></span> |  |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="926fb-191">Özellik</span><span class="sxs-lookup"><span data-stu-id="926fb-191">Property</span></span> | <span data-ttu-id="926fb-192">Açıklama</span><span class="sxs-lookup"><span data-stu-id="926fb-192">Description</span></span> | <span data-ttu-id="926fb-193">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="926fb-193">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="926fb-194">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="926fb-194">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="926fb-195">`HttpContext.Request.Body` Ve`HttpContext.Response.Body`için zaman uyumlu giriş/çıkışa izin verilip verilmeyeceğini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="926fb-195">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
| [<span data-ttu-id="926fb-196">Authentication. AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="926fb-196">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="926fb-197">Anonim isteklere izin verin.</span><span class="sxs-lookup"><span data-stu-id="926fb-197">Allow anonymous requests.</span></span> | `true` |
| [<span data-ttu-id="926fb-198">Authentication. düzenleri</span><span class="sxs-lookup"><span data-stu-id="926fb-198">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="926fb-199">İzin verilen kimlik doğrulama düzenlerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="926fb-199">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="926fb-200">Dinleyici elden atılırken önce herhangi bir zamanda değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-200">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="926fb-201">Değerler [authenticationdüzenlerinin numaralandırması](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes)tarafından sağlanır: `Basic`, `Negotiate` `Kerberos`, `None`,, ve `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="926fb-201">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
| [<span data-ttu-id="926fb-202">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="926fb-202">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="926fb-203">Uygun üst bilgileri içeren yanıtlar için [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) önbelleğe almayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="926fb-203">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="926fb-204">Yanıt, `Set-Cookie` `Vary`veya üstbilgileriiçermeyebilir.`Pragma`</span><span class="sxs-lookup"><span data-stu-id="926fb-204">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="926fb-205">`Cache-Control` Bir`shared-max-age`ya dadeğeri`Expires` ya da bir üst bilgi içermelidir. `max-age` `public`</span><span class="sxs-lookup"><span data-stu-id="926fb-205">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="926fb-206">En fazla eşzamanlı kabul sayısı.</span><span class="sxs-lookup"><span data-stu-id="926fb-206">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="926fb-207">5 &times; [ortam.<br> ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="926fb-207">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="926fb-208">Kabul edilecek eşzamanlı bağlantı sayısı üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="926fb-208">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="926fb-209">Sonsuz `-1` için kullanın.</span><span class="sxs-lookup"><span data-stu-id="926fb-209">Use `-1` for infinite.</span></span> <span data-ttu-id="926fb-210">Kayıt `null` defterinin makine genelindeki ayarını kullanmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="926fb-210">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="926fb-211">sayısız</span><span class="sxs-lookup"><span data-stu-id="926fb-211">(unlimited)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="926fb-212"><a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="926fb-212">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="926fb-213">30000000 bayt</span><span class="sxs-lookup"><span data-stu-id="926fb-213">30000000 bytes</span></span><br><span data-ttu-id="926fb-214">(~ 28,6 MB)</span><span class="sxs-lookup"><span data-stu-id="926fb-214">(~28.6 MB)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="926fb-215">Sıraya alınabilen en fazla istek sayısı.</span><span class="sxs-lookup"><span data-stu-id="926fb-215">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="926fb-216">1000</span><span class="sxs-lookup"><span data-stu-id="926fb-216">1000</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="926fb-217">İstemci bağlantısının kesilmesinden kaynaklanan yanıt gövdesi yazmasının, özel durumlar oluşturması veya normal şekilde tamamlanması gerektiğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="926fb-217">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="926fb-218">(normal olarak)</span><span class="sxs-lookup"><span data-stu-id="926fb-218">(complete normally)</span></span> |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="926fb-219">Kayıt defterinde da yapılandırılabilen http <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> . sys yapılandırmasını kullanıma sunun.</span><span class="sxs-lookup"><span data-stu-id="926fb-219">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="926fb-220">Varsayılan değerler de dahil olmak üzere her bir ayar hakkında daha fazla bilgi edinmek için API bağlantılarını izleyin:</span><span class="sxs-lookup"><span data-stu-id="926fb-220">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="926fb-221">HTTP sunucusu API 'sinin varlık gövdesini etkin tut bağlantısı üzerinde boşaltmasına izin verilen [TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; süresi.</span><span class="sxs-lookup"><span data-stu-id="926fb-221">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="926fb-222">[TimeoutManager. entitybody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; zamanına, istek varlığı gövdesinin gelmesi için izin verildi.</span><span class="sxs-lookup"><span data-stu-id="926fb-222">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="926fb-223">HTTP sunucusu API 'sinin istek üst bilgisini ayrıştırması için [TimeoutManager. headerwaıt](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; zamanına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-223">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="926fb-224">[TimeoutManager. boştaki bir bağlantı için ıdletimeout bağlantı](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; zamanına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-224">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="926fb-225">[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; yanıt için en düşük gönderme hızı.</span><span class="sxs-lookup"><span data-stu-id="926fb-225">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="926fb-226">[TimeoutManager.](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; isteğin, istek sırasında, uygulamanın onu çekmeden önce kalmasına izin verilen RequestQueue süresi.</span><span class="sxs-lookup"><span data-stu-id="926fb-226">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="926fb-227">HTTP. sys ile kaydolmak içinöğesinibelirtin.<xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection></span><span class="sxs-lookup"><span data-stu-id="926fb-227">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="926fb-228">En yararlı olan [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), koleksiyona bir ön ek eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926fb-228">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="926fb-229">Bunlar, dinleyici elden atılıyor öncesinde herhangi bir zamanda değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-229">These may be modified at any time prior to disposing the listener.</span></span> |  |

::: moniker-end

<a name="maxrequestbodysize"></a>

<span data-ttu-id="926fb-230">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="926fb-230">**MaxRequestBodySize**</span></span>

<span data-ttu-id="926fb-231">Herhangi bir istek gövdesinin bayt olarak izin verilen en büyük boyutu.</span><span class="sxs-lookup"><span data-stu-id="926fb-231">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="926fb-232">Olarak `null`ayarlandığında, en büyük istek gövdesi boyutu sınırsızdır.</span><span class="sxs-lookup"><span data-stu-id="926fb-232">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="926fb-233">Bu sınırın, her zaman sınırsız olan yükseltilmiş bağlantıları üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="926fb-233">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

<span data-ttu-id="926fb-234">Tek `IActionResult` bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="926fb-234">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="926fb-235">Uygulama isteği okumayı başlattıktan sonra bir istek üzerindeki sınırı yapılandırmayı denerse bir özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="926fb-235">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="926fb-236">Özelliğin `IsReadOnly` salt okuma durumunda olup `MaxRequestBodySize` olmadığını göstermek için bir özellik kullanılabilir, yani sınırı yapılandırmak için çok geç.</span><span class="sxs-lookup"><span data-stu-id="926fb-236">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="926fb-237">Uygulamanın istek başına geçersiz kılması <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> gerekiyorsa, <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>şunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="926fb-237">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

<span data-ttu-id="926fb-238">Visual Studio kullanıyorsanız, uygulamanın IIS veya IIS Express çalıştıracak şekilde yapılandırılmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="926fb-238">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="926fb-239">Visual Studio 'da varsayılan başlatma profili IIS Express içindir.</span><span class="sxs-lookup"><span data-stu-id="926fb-239">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="926fb-240">Projeyi konsol uygulaması olarak çalıştırmak için, aşağıdaki ekran görüntüsünde gösterildiği gibi seçili profili el ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="926fb-240">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

![Konsol uygulaması profilini seçin](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="926fb-242">Windows Server 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="926fb-242">Configure Windows Server</span></span>

1. <span data-ttu-id="926fb-243">Uygulamanın açmak için bağlantı noktalarını belirleme ve [Windows Güvenlik Duvarı](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) 'Nı veya [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell CMDLET 'ini kullanarak trafiğin http. sys ' ye ulaşmasını sağlamak için güvenlik duvarı bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="926fb-243">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="926fb-244">Aşağıdaki komutlarda ve uygulama yapılandırmasında, 443 numaralı bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926fb-244">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="926fb-245">Bir Azure sanal makinesine dağıtım yaparken, [ağ güvenlik grubundaki](/azure/virtual-machines/windows/nsg-quickstart-portal)bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="926fb-245">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="926fb-246">Aşağıdaki komutlarda ve uygulama yapılandırmasında, 443 numaralı bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926fb-246">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="926fb-247">Gerekirse, X. 509.440 sertifikalarını edinin ve yükler.</span><span class="sxs-lookup"><span data-stu-id="926fb-247">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="926fb-248">Windows 'da, [New-SelfSignedCertificate PowerShell cmdlet 'ini](/powershell/module/pkiclient/new-selfsignedcertificate)kullanarak otomatik olarak imzalanan sertifikalar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="926fb-248">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="926fb-249">Desteklenmeyen bir örnek için bkz [. UpdateIISExpressSSLForChrome. ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span><span class="sxs-lookup"><span data-stu-id="926fb-249">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="926fb-250">Sunucunun **yerel makine** > **Kişisel** deposunda otomatik olarak imzalanan veya CA imzalı sertifikalar yükler.</span><span class="sxs-lookup"><span data-stu-id="926fb-250">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="926fb-251">Uygulama [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise, .net core, .NET Framework veya her ikisini de (uygulama .NET Framework hedefleyen bir .NET Core uygulaması ise) yükler.</span><span class="sxs-lookup"><span data-stu-id="926fb-251">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="926fb-252">**.NET Core** Uygulama .NET Core gerektiriyorsa .NET Core [indirmelerinde](https://dotnet.microsoft.com/download) **.NET Core çalışma zamanı** yükleyicisini edinin ve çalıştırın. &ndash;</span><span class="sxs-lookup"><span data-stu-id="926fb-252">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="926fb-253">Tam SDK 'Yı sunucuya yüklemeyin.</span><span class="sxs-lookup"><span data-stu-id="926fb-253">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="926fb-254">**.NET Framework** Uygulama .NET Framework gerektiriyorsa, [.NET Framework yükleme kılavuzuna](/dotnet/framework/install/)bakın. &ndash;</span><span class="sxs-lookup"><span data-stu-id="926fb-254">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="926fb-255">Gerekli .NET Framework yüklemesi.</span><span class="sxs-lookup"><span data-stu-id="926fb-255">Install the required .NET Framework.</span></span> <span data-ttu-id="926fb-256">En son .NET Framework yükleyicisi [.NET Core İndirmeleri](https://dotnet.microsoft.com/download) sayfasından edinilebilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-256">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="926fb-257">Uygulama, [kendi içinde bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise, uygulama çalışma zamanını dağıtımda içerir.</span><span class="sxs-lookup"><span data-stu-id="926fb-257">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="926fb-258">Sunucuda çerçeve yüklemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="926fb-258">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="926fb-259">Uygulamada URL 'Leri ve bağlantı noktalarını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="926fb-259">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="926fb-260">Varsayılan olarak, ASP.NET Core öğesine `http://localhost:5000`bağlanır.</span><span class="sxs-lookup"><span data-stu-id="926fb-260">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="926fb-261">URL öneklerini ve bağlantı noktalarını yapılandırmak için seçenekler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="926fb-261">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="926fb-262">`urls`komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="926fb-262">`urls` command-line argument</span></span>
   * <span data-ttu-id="926fb-263">`ASPNETCORE_URLS`ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="926fb-263">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="926fb-264">Aşağıdaki kod örneği, 443 numaralı bağlantı noktasında <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> sunucunun yerel IP adresi `10.0.0.4` ile nasıl kullanılacağını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="926fb-264">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

::: moniker range=">= aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/3.x/Program.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/2.x/Program.cs?highlight=6)]

::: moniker-end

   <span data-ttu-id="926fb-265">Avantajı `UrlPrefixes` , hatalı biçimli ön ekler için hemen bir hata iletisi oluşturulmasından oluşur.</span><span class="sxs-lookup"><span data-stu-id="926fb-265">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="926fb-266">`UseUrls` Ayarlarıgeçersizkıl`urls` . / `UrlPrefixes` / `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="926fb-266">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="926fb-267">Bu nedenle, ve `UseUrls` `ASPNETCORE_URLS` ortam değişkeninin `urls`avantajı Kestrel ve http. sys arasında geçiş yapmak daha kolay olabilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-267">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span>

   <span data-ttu-id="926fb-268">HTTP. sys, [http sunucusu API UrlPrefix dize biçimlerini](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)kullanır.</span><span class="sxs-lookup"><span data-stu-id="926fb-268">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="926fb-269">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926fb-269">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="926fb-270">Üst düzey joker karakter bağlamaları uygulama güvenliği güvenlik açıklarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="926fb-270">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="926fb-271">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="926fb-271">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="926fb-272">Joker karakterler yerine açık konak adlarını veya IP adreslerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="926fb-272">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="926fb-273">Alt etki alanı joker bağlantısı (örneğin `*.mysub.com`,), tüm üst etki alanını (Bu güvenlik açığı olan `*.com`aksine) kontrol ediyorsanız bir güvenlik riski değildir.</span><span class="sxs-lookup"><span data-stu-id="926fb-273">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="926fb-274">Daha fazla bilgi için bkz [. RFC 7230: Bölüm 5,4: Ana](https://tools.ietf.org/html/rfc7230#section-5.4)bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="926fb-274">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="926fb-275">Ön ek, sunucudaki URL öneklerini ister.</span><span class="sxs-lookup"><span data-stu-id="926fb-275">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="926fb-276">HTTP. sys ' yi yapılandırmaya yönelik yerleşik araç *netsh. exe*' dir.</span><span class="sxs-lookup"><span data-stu-id="926fb-276">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="926fb-277">*netsh. exe* , URL öneklerini ayırmak ve X. 509.440 sertifikaları atamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926fb-277">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="926fb-278">Araç, yönetici ayrıcalıkları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="926fb-278">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="926fb-279">Uygulamanın URL 'Lerini kaydettirmek için *netsh. exe* aracını kullanın:</span><span class="sxs-lookup"><span data-stu-id="926fb-279">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="926fb-280">`<URL>`&ndash; Tam nitelikli Tekdüzen Kaynak Bulucu (URL).</span><span class="sxs-lookup"><span data-stu-id="926fb-280">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="926fb-281">Joker karakter bağlama kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="926fb-281">Don't use a wildcard binding.</span></span> <span data-ttu-id="926fb-282">Geçerli bir ana bilgisayar adı veya yerel IP adresi kullanın.</span><span class="sxs-lookup"><span data-stu-id="926fb-282">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="926fb-283">*URL, sondaki eğik çizgi içermelidir.*</span><span class="sxs-lookup"><span data-stu-id="926fb-283">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="926fb-284">`<USER>`&ndash; Kullanıcı veya Kullanıcı grubu adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="926fb-284">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="926fb-285">Aşağıdaki örnekte sunucusunun `10.0.0.4`yerel IP adresi:</span><span class="sxs-lookup"><span data-stu-id="926fb-285">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="926fb-286">Bir URL kaydedildiğinde, araç ile `URL reservation successfully added`yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="926fb-286">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="926fb-287">Kayıtlı bir URL 'yi silmek için şu `delete urlacl` komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="926fb-287">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="926fb-288">X. 509.440 sertifikalarını sunucusuna kaydedin.</span><span class="sxs-lookup"><span data-stu-id="926fb-288">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="926fb-289">Uygulamanın sertifikalarını kaydetmek için *netsh. exe* aracını kullanın:</span><span class="sxs-lookup"><span data-stu-id="926fb-289">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="926fb-290">`<IP>`&ndash; Bağlama için yerel IP adresini belirtir.</span><span class="sxs-lookup"><span data-stu-id="926fb-290">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="926fb-291">Joker karakter bağlama kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="926fb-291">Don't use a wildcard binding.</span></span> <span data-ttu-id="926fb-292">Geçerli bir IP adresi kullanın.</span><span class="sxs-lookup"><span data-stu-id="926fb-292">Use a valid IP address.</span></span>
   * <span data-ttu-id="926fb-293">`<PORT>`&ndash; Bağlama için bağlantı noktasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="926fb-293">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="926fb-294">`<THUMBPRINT>`&ndash; X. 509.440 sertifikası parmak izi.</span><span class="sxs-lookup"><span data-stu-id="926fb-294">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="926fb-295">`<GUID>`&ndash; Uygulamayı bilgilendirme amacıyla temsil eden geliştirici tarafından oluşturulan GUID.</span><span class="sxs-lookup"><span data-stu-id="926fb-295">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="926fb-296">Başvuru amacıyla, GUID 'yi uygulamada bir paket etiketi olarak depolayın:</span><span class="sxs-lookup"><span data-stu-id="926fb-296">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="926fb-297">Visual Studio'da:</span><span class="sxs-lookup"><span data-stu-id="926fb-297">In Visual Studio:</span></span>
     * <span data-ttu-id="926fb-298">**Çözüm Gezgini** ' de uygulamaya sağ tıklayıp **Özellikler**' i seçerek uygulamanın proje özelliklerini açın.</span><span class="sxs-lookup"><span data-stu-id="926fb-298">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="926fb-299">**Paket** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="926fb-299">Select the **Package** tab.</span></span>
     * <span data-ttu-id="926fb-300">**Etiketler** alanında oluşturduğunuz GUID 'yi girin.</span><span class="sxs-lookup"><span data-stu-id="926fb-300">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="926fb-301">Visual Studio kullanmadığınız durumlarda:</span><span class="sxs-lookup"><span data-stu-id="926fb-301">When not using Visual Studio:</span></span>
     * <span data-ttu-id="926fb-302">Uygulamanın proje dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="926fb-302">Open the app's project file.</span></span>
     * <span data-ttu-id="926fb-303">Oluşturduğunuz GUID ile yeni veya var olan `<PropertyGroup>` bir özelliğiekleyin:`<PackageTags>`</span><span class="sxs-lookup"><span data-stu-id="926fb-303">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="926fb-304">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="926fb-304">In the following example:</span></span>

   * <span data-ttu-id="926fb-305">Sunucunun yerel IP adresi `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="926fb-305">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="926fb-306">Bir çevrimiçi rastgele GUID Oluşturucu `appid` değeri sağlar.</span><span class="sxs-lookup"><span data-stu-id="926fb-306">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="926fb-307">Bir sertifika kaydedildiğinde, araç ile `SSL Certificate successfully added`yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="926fb-307">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="926fb-308">Bir sertifika kaydını silmek için şu `delete sslcert` komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="926fb-308">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="926fb-309">*Netsh. exe*için başvuru belgeleri:</span><span class="sxs-lookup"><span data-stu-id="926fb-309">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="926fb-310">Köprü Metni Aktarım Protokolü (HTTP) için Netsh komutları</span><span class="sxs-lookup"><span data-stu-id="926fb-310">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="926fb-311">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="926fb-311">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="926fb-312">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="926fb-312">Run the app.</span></span>

   <span data-ttu-id="926fb-313">1024 'den büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak localhost 'a bağlanırken uygulamayı çalıştırmak için yönetici ayrıcalıklarına gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="926fb-313">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="926fb-314">Diğer yapılandırmalarda (örneğin, yerel bir IP adresi veya 443 numaralı bağlantı noktasına bağlama), uygulamayı yönetici ayrıcalıklarıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="926fb-314">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="926fb-315">Uygulama, sunucunun genel IP adresinde yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="926fb-315">The app responds at the server's public IP address.</span></span> <span data-ttu-id="926fb-316">Bu örnekte, sunucusuna, genel IP adresinde `104.214.79.47`Internet 'ten ulaşılırsa.</span><span class="sxs-lookup"><span data-stu-id="926fb-316">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="926fb-317">Bu örnekte bir geliştirme sertifikası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="926fb-317">A development certificate is used in this example.</span></span> <span data-ttu-id="926fb-318">Tarayıcının güvenilmeyen sertifika uyarısı atlandıktan sonra sayfa güvenli bir şekilde yüklenir.</span><span class="sxs-lookup"><span data-stu-id="926fb-318">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![Uygulamanın dizin sayfasını yüklü olarak gösteren tarayıcı penceresi](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="926fb-320">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="926fb-320">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="926fb-321">Internet veya şirket ağından gelen isteklerle etkileşime geçen HTTP. sys tarafından barındırılan uygulamalar için, proxy sunucularının ve yük dengeleyiciler 'nin arkasında barındırılırken ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="926fb-321">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="926fb-322">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="926fb-322">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="926fb-323">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="926fb-323">Additional resources</span></span>

* [<span data-ttu-id="926fb-324">HTTP. sys ile Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="926fb-324">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#httpsys)
* [<span data-ttu-id="926fb-325">HTTP Sunucusu API 'SI</span><span class="sxs-lookup"><span data-stu-id="926fb-325">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="926fb-326">ASPNET/HttpSysServer GitHub deposu (kaynak kodu)</span><span class="sxs-lookup"><span data-stu-id="926fb-326">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="926fb-327">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="926fb-327">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
