---
title: ASP.NET core'da HTTP.sys web sunucusu uygulaması
author: guardrex
description: HTTP.sys, ASP.NET Core, Windows için bir web sunucusu hakkında bilgi edinin. HTTP.sys çekirdek modu sürücüsü üzerinde oluşturulmuş, HTTP.sys için IIS olmadan İnternet'e doğrudan bağlantı için kullanılan Kestrel alternatiftir.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/21/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 6ec9b7bf3da0015b8ac3918a4d47644fffc14cdb
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209180"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="4a97c-104">ASP.NET core'da HTTP.sys web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="4a97c-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="4a97c-105">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4a97c-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4a97c-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) olduğu bir [ASP.NET Core web sunucusu](xref:fundamentals/servers/index) Windows üzerinde yalnızca çalışır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="4a97c-107">HTTP.sys olan alternatif [Kestrel](xref:fundamentals/servers/kestrel) sunucu ve teklifler bazı özellikleri Kestrel sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="4a97c-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a97c-108">HTTP.sys ile uyumlu olmayan [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve IIS veya IIS Express ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4a97c-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="4a97c-109">HTTP.sys aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="4a97c-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="4a97c-110">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4a97c-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="4a97c-111">Bağlantı noktası paylaşma</span><span class="sxs-lookup"><span data-stu-id="4a97c-111">Port sharing</span></span>
* <span data-ttu-id="4a97c-112">SNI ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="4a97c-112">HTTPS with SNI</span></span>
* <span data-ttu-id="4a97c-113">HTTP/2 üzerinden TLS (Windows 10 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="4a97c-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="4a97c-114">Doğrudan bir dosya aktarımı</span><span class="sxs-lookup"><span data-stu-id="4a97c-114">Direct file transmission</span></span>
* <span data-ttu-id="4a97c-115">Yanıtları Önbelleğe Alma</span><span class="sxs-lookup"><span data-stu-id="4a97c-115">Response caching</span></span>
* <span data-ttu-id="4a97c-116">WebSockets (Windows 8 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="4a97c-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="4a97c-117">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="4a97c-117">Supported Windows versions:</span></span>

* <span data-ttu-id="4a97c-118">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4a97c-118">Windows 7 or later</span></span>
* <span data-ttu-id="4a97c-119">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4a97c-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="4a97c-120">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a97c-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="4a97c-121">HTTP.sys kullanıldığı durumlar</span><span class="sxs-lookup"><span data-stu-id="4a97c-121">When to use HTTP.sys</span></span>

<span data-ttu-id="4a97c-122">HTTP.sys dağıtımları için yararlıdır burada:</span><span class="sxs-lookup"><span data-stu-id="4a97c-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="4a97c-123">IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak için bir gereksinim yoktur.</span><span class="sxs-lookup"><span data-stu-id="4a97c-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys doğrudan Internet ile iletişim kurar.](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="4a97c-125">Bir iç dağıtım Kestrel içinde kullanılabilir değil bir özellik gibi gerektirir [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="4a97c-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys iç ağa ile doğrudan iletişim kurar.](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="4a97c-127">HTTP.sys, birçok türde saldırılara karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="4a97c-128">IIS'nin, HTTP.sys üzerine bir HTTP dinleyicisi olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="4a97c-129">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="4a97c-129">HTTP/2 support</span></span>

<span data-ttu-id="4a97c-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki gereksinimleri dayandırırsanız ASP.NET Core uygulamaları karşılanması için etkin:</span><span class="sxs-lookup"><span data-stu-id="4a97c-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="4a97c-131">Windows Server 2016/Windows 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4a97c-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="4a97c-132">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı</span><span class="sxs-lookup"><span data-stu-id="4a97c-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="4a97c-133">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="4a97c-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4a97c-134">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4a97c-135">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="4a97c-136">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="4a97c-137">Bir HTTP/2 bağlantı değil, bağlantı, HTTP/1.1 geri döner.</span><span class="sxs-lookup"><span data-stu-id="4a97c-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="4a97c-138">Gelecekteki bir Windows sürümünde, HTTP/2 yapılandırma bayrakları HTTP/2 HTTP.sys ile devre dışı bırakma olanağı dahil olmak üzere, kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="4a97c-139">Çekirdek modu kimlik doğrulamasını Kerberos ile</span><span class="sxs-lookup"><span data-stu-id="4a97c-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="4a97c-140">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü HTTP.sys temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4a97c-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="4a97c-141">Kullanıcı modu kimlik doğrulaması, Kerberos ve HTTP.sys ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4a97c-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="4a97c-142">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="4a97c-143">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="4a97c-144">HTTP.sys kullanma</span><span class="sxs-lookup"><span data-stu-id="4a97c-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="4a97c-145">HTTP.sys kullanmak için ASP.NET Core uygulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a97c-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="4a97c-146">Proje dosyasındaki bir paket başvurusu kullanırken gerekli değildir [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="4a97c-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="4a97c-147">Değil kullanırken `Microsoft.AspNetCore.App` metapackage, paket başvurusu ekleme [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span><span class="sxs-lookup"><span data-stu-id="4a97c-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="4a97c-148">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> Web ana bilgisayarı, gerekli belirtme oluştururken genişletme yöntemi <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span><span class="sxs-lookup"><span data-stu-id="4a97c-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building Web Host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="4a97c-149">Ek HTTP.sys yapılandırma aracılığıyla işlenir [kayıt defteri ayarları](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span><span class="sxs-lookup"><span data-stu-id="4a97c-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="4a97c-150">**HTTP.sys seçenekleri**</span><span class="sxs-lookup"><span data-stu-id="4a97c-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="4a97c-151">Özellik</span><span class="sxs-lookup"><span data-stu-id="4a97c-151">Property</span></span> | <span data-ttu-id="4a97c-152">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4a97c-152">Description</span></span> | <span data-ttu-id="4a97c-153">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="4a97c-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="4a97c-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="4a97c-154">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="4a97c-155">Zaman uyumlu giriş/çıkış için izin verilip verilmediğini `HttpContext.Request.Body` ve `HttpContext.Response.Body`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="4a97c-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="4a97c-156">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="4a97c-157">Anonim isteklere izin verin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="4a97c-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="4a97c-158">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="4a97c-159">İzin verilen kimlik doğrulama şemasını belirtin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="4a97c-160">Dinleyici disposing önce herhangi bir zamanda değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="4a97c-161">Değerleri tarafından sağlanan [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, ve `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-161">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="4a97c-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="4a97c-162">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="4a97c-163">Girişimi [çekirdek modu](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) uygun üst bilgilerini içeren yanıtlar için önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="4a97c-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="4a97c-164">Yanıta dahil `Set-Cookie`, `Vary`, veya `Pragma` üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="4a97c-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="4a97c-165">İçermesi gerekir bir `Cache-Control` üst bilgisi bu `public` ve ya da bir `shared-max-age` veya `max-age` değeri veya bir `Expires` başlığı.</span><span class="sxs-lookup"><span data-stu-id="4a97c-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="4a97c-166">En fazla eşzamanlı sayısı kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4a97c-166">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="4a97c-167">5 &times; [ortam.<br> ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="4a97c-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="4a97c-168">Kabul etmek üzere kullanılan eşzamanlı bağlantı sayısı.</span><span class="sxs-lookup"><span data-stu-id="4a97c-168">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="4a97c-169">Kullanım `-1` sonsuz için.</span><span class="sxs-lookup"><span data-stu-id="4a97c-169">Use `-1` for infinite.</span></span> <span data-ttu-id="4a97c-170">Kullanma `null` kayıt defterinin makine genelindeki ayarını kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="4a97c-170">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="4a97c-171">(sınırsız)</span><span class="sxs-lookup"><span data-stu-id="4a97c-171">(unlimited)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="4a97c-172">Bkz: <a href="#maxrequestbodysize">MaxRequestBodySize</a> bölümü.</span><span class="sxs-lookup"><span data-stu-id="4a97c-172">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="4a97c-173">30000000 bayt</span><span class="sxs-lookup"><span data-stu-id="4a97c-173">30000000 bytes</span></span><br><span data-ttu-id="4a97c-174">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="4a97c-174">(~28.6 MB)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="4a97c-175">Sıraya alınabilecek isteklerinin sayısı.</span><span class="sxs-lookup"><span data-stu-id="4a97c-175">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="4a97c-176">1000</span><span class="sxs-lookup"><span data-stu-id="4a97c-176">1000</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="4a97c-177">Yanıt gövdesi yazma, hata nedeniyle istemci bağlantısını keser veya özel durumlar genellikle tamamlamak gösterir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-177">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="4a97c-178">(normalde Tamamla)</span><span class="sxs-lookup"><span data-stu-id="4a97c-178">(complete normally)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="4a97c-179">HTTP.sys kullanıma <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> yapılandırma, kayıt defterinde da yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-179">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="4a97c-180">Varsayılan değerleri dahil olmak üzere, her ayar hakkında daha fazla bilgi için API bağlantıları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4a97c-180">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="4a97c-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP Sunucusu API varlık gövdesini bir canlı bağlantı boşaltma zaman izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="4a97c-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; ulaşması istek Varlık gövdesi için izin verilen süresi.</span><span class="sxs-lookup"><span data-stu-id="4a97c-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="4a97c-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; süresi izin verilen HTTP Sunucusu API istek üstbilgisi ayrıştırılamıyor.</span><span class="sxs-lookup"><span data-stu-id="4a97c-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="4a97c-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; boştaki bir bağlantının için izin verilen süresi.</span><span class="sxs-lookup"><span data-stu-id="4a97c-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="4a97c-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; en düşük yanıt hızı gönderin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="4a97c-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; zaman izin isteğinin, uygulamayı alır önce isteği kuyruğunda kalır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="4a97c-187">Belirtin <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> HTTP.sys ile kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="4a97c-187">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="4a97c-188">Kullanışlı [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), bir önek koleksiyona eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-188">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="4a97c-189">Bu dinleyici disposing önce herhangi bir zamanda değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-189">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>

   <span data-ttu-id="4a97c-190">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="4a97c-190">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="4a97c-191">Bayt cinsinden izin verilen maksimum herhangi bir istek gövdesi boyutu.</span><span class="sxs-lookup"><span data-stu-id="4a97c-191">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="4a97c-192">Ayarlandığında `null`, en fazla istek gövdesi boyutu sınırsızdır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-192">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="4a97c-193">Bu sınırı her zaman sınırsız yükseltilen bağlantılar üzerinde etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4a97c-193">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="4a97c-194">Bir ASP.NET Core MVC uygulaması için tek bir sınırı geçersiz kılmak için önerilen yöntem `IActionResult` kullanmaktır <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliği bir eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4a97c-194">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="4a97c-195">İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırını yapılandırmak uygulamanın çalışırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4a97c-195">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="4a97c-196">Bir `IsReadOnly` özelliği belirtmek için kullanılabilir `MaxRequestBodySize` özelliği olan bir salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-196">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="4a97c-197">Uygulamayı'ı geçersiz kılmalıdır varsa <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> istek başına, kullanın <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span><span class="sxs-lookup"><span data-stu-id="4a97c-197">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="4a97c-198">Visual Studio kullanıyorsanız, uygulamayı IIS veya IIS Express çalıştırmak için yapılandırılmamış emin olun.</span><span class="sxs-lookup"><span data-stu-id="4a97c-198">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="4a97c-199">Visual Studio'da varsayılan başlatma için IIS Express profilidir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-199">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="4a97c-200">Proje bir konsol uygulaması çalıştırmak için el ile seçilen profil, aşağıdaki ekran görüntüsünde gösterildiği gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4a97c-200">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![Konsol uygulama profili seçin](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="4a97c-202">Windows Server'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a97c-202">Configure Windows Server</span></span>

1. <span data-ttu-id="4a97c-203">Uygulama ve kullanımı için açılacak bağlantı noktaları belirleme [Windows Güvenlik Duvarı](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) veya [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) trafiğin HTTP.sys ulaşmasına izin vermek için güvenlik duvarı bağlantı noktalarını açmak için PowerShell cmdlet'i.</span><span class="sxs-lookup"><span data-stu-id="4a97c-203">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="4a97c-204">Aşağıdaki komutları ve uygulama yapılandırması, 443 numaralı bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-204">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="4a97c-205">Bir Azure VM Dağıtım yaparken, bağlantı noktaları açma [ağ güvenlik grubu](/azure/virtual-machines/windows/nsg-quickstart-portal).</span><span class="sxs-lookup"><span data-stu-id="4a97c-205">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="4a97c-206">Aşağıdaki komutları ve uygulama yapılandırması, 443 numaralı bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-206">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="4a97c-207">Edinin ve gerektiğinde X.509 sertifikaları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-207">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="4a97c-208">Otomatik olarak imzalanan sertifikaları kullanarak Windows üzerinde oluşturun [New-SelfSignedCertificate PowerShell cmdlet'i](/powershell/module/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="4a97c-208">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="4a97c-209">Desteklenmeyen bir örnek için bkz [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span><span class="sxs-lookup"><span data-stu-id="4a97c-209">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="4a97c-210">Sunucunun otomatik olarak imzalanan veya CA imzalı sertifika yükleme **yerel makine** > **kişisel** depolayın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-210">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="4a97c-211">Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core, .NET Framework veya her ikisi de (uygulama .NET Framework'ü hedefleyen bir .NET Core uygulaması ise) yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-211">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="4a97c-212">**.NET core** &ndash; uygulama, .NET Core gerektiriyorsa, edinme ve çalıştırma **.NET Core çalışma zamanı** Yükleyicisi'nden [.NET Core indirir](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="4a97c-212">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="4a97c-213">SDK tamamını sunucuda yüklemeyin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-213">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="4a97c-214">**.NET framework** &ndash; uygulama .NET Framework gerekiyorsa, bkz. [.NET Framework Yükleme Kılavuzu](/dotnet/framework/install/).</span><span class="sxs-lookup"><span data-stu-id="4a97c-214">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="4a97c-215">Gerekli .NET Framework'ü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-215">Install the required .NET Framework.</span></span> <span data-ttu-id="4a97c-216">En son .NET Framework yükleyicisi [.NET Core indirir](https://dotnet.microsoft.com/download) sayfası.</span><span class="sxs-lookup"><span data-stu-id="4a97c-216">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="4a97c-217">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-scd), uygulama çalışma zamanı, dağıtımda içerir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-217">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="4a97c-218">Sunucuda hiçbir framework yüklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-218">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="4a97c-219">Uygulama URL'lerini ve bağlantı noktalarını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-219">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="4a97c-220">Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-220">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="4a97c-221">URL ön ekleri ve bağlantı noktalarını yapılandırmak için Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4a97c-221">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="4a97c-222">`urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="4a97c-222">`urls` command-line argument</span></span>
   * <span data-ttu-id="4a97c-223">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="4a97c-223">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="4a97c-224">Aşağıdaki kod örneği kullanma işlemini gösterir <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> sunucunun yerel IP adresi ile `10.0.0.4` 443 numaralı bağlantı noktasında:</span><span class="sxs-lookup"><span data-stu-id="4a97c-224">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="4a97c-225">Bir avantajı `UrlPrefixes` bir hata iletisi hemen düzgün biçimlendirilmemiş ön ekleri için oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="4a97c-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="4a97c-226">Ayarlarında `UrlPrefixes` geçersiz kılma `UseUrls` / `urls` / `ASPNETCORE_URLS` ayarları.</span><span class="sxs-lookup"><span data-stu-id="4a97c-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="4a97c-227">Bu nedenle, bir avantajı `UseUrls`, `urls`ve `ASPNETCORE_URLS` ortam değişkenidir Kestrel ve HTTP.sys arasında geçiş yapmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="4a97c-228">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="4a97c-228">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="4a97c-229">HTTP.sys kullanan [HTTP Sunucusu API UrlPrefix dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a97c-229">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="4a97c-230">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-230">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="4a97c-231">Üst düzey joker bağlamaları, güvenlik açıklarını uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4a97c-231">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="4a97c-232">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-232">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="4a97c-233">Açık bir ana bilgisayar adlarını veya IP adresleri yerine joker karakterler kullanın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-233">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="4a97c-234">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetleyen bir güvenlik riski değildir (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="4a97c-234">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="4a97c-235">Daha fazla bilgi için [RFC 7230: Bölüm 5.4: Konak](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="4a97c-235">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="4a97c-236">URL ön ekleri sunucuda preregister.</span><span class="sxs-lookup"><span data-stu-id="4a97c-236">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="4a97c-237">HTTP.sys yapılandırmak için yerleşik bir aracı *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="4a97c-237">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="4a97c-238">*Netsh.exe* URL ön ekleri ayırabilir ve X.509 sertifikaları atamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-238">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="4a97c-239">Aracı yöneticisi ayrıcalıkları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-239">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="4a97c-240">Kullanım *netsh.exe* aracı uygulama için URL'leri kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="4a97c-240">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="4a97c-241">`<URL>` &ndash; Tam Tekdüzen Kaynak Konum Belirleyicisi (URL).</span><span class="sxs-lookup"><span data-stu-id="4a97c-241">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="4a97c-242">Joker karakter bağlama kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-242">Don't use a wildcard binding.</span></span> <span data-ttu-id="4a97c-243">Geçerli ana bilgisayar adı veya yerel IP adresi kullanın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-243">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="4a97c-244">*URL'nin sonunda bir eğik çizgi içermelidir.*</span><span class="sxs-lookup"><span data-stu-id="4a97c-244">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="4a97c-245">`<USER>` &ndash; Kullanıcı veya kullanıcı grubunun adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-245">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="4a97c-246">Aşağıdaki örnekte, yerel sunucunun IP adresi olan `10.0.0.4`:</span><span class="sxs-lookup"><span data-stu-id="4a97c-246">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="4a97c-247">Bir URL kaydedildiğinde, bu aracı ile yanıt veren `URL reservation successfully added`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-247">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="4a97c-248">Kayıtlı bir URL silmek için kullanın `delete urlacl` komutu:</span><span class="sxs-lookup"><span data-stu-id="4a97c-248">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="4a97c-249">X.509 sertifikaları sunucusuna kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4a97c-249">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="4a97c-250">Kullanım *netsh.exe* aracı uygulama için sertifikaları kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="4a97c-250">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="4a97c-251">`<IP>` &ndash; Bağlama için yerel IP adresi belirtir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-251">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="4a97c-252">Joker karakter bağlama kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-252">Don't use a wildcard binding.</span></span> <span data-ttu-id="4a97c-253">Geçerli bir IP adresi kullanın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-253">Use a valid IP address.</span></span>
   * <span data-ttu-id="4a97c-254">`<PORT>` &ndash; Bağlama için bir bağlantı noktasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-254">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="4a97c-255">`<THUMBPRINT>` &ndash; X.509 sertifika parmak izi.</span><span class="sxs-lookup"><span data-stu-id="4a97c-255">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="4a97c-256">`<GUID>` &ndash; Bilgilendirme amacıyla uygulamayı temsil etmek için bir geliştirici tarafından oluşturulan GUID.</span><span class="sxs-lookup"><span data-stu-id="4a97c-256">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="4a97c-257">Başvuru amacıyla GUID uygulamada bir paket etiket olarak depola:</span><span class="sxs-lookup"><span data-stu-id="4a97c-257">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="4a97c-258">Visual Studio'da:</span><span class="sxs-lookup"><span data-stu-id="4a97c-258">In Visual Studio:</span></span>
     * <span data-ttu-id="4a97c-259">Uygulamada sağ tıklayarak uygulamanın proje özelliklerini açın **Çözüm Gezgini** seçerek **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="4a97c-259">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="4a97c-260">Seçin **paket** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="4a97c-260">Select the **Package** tab.</span></span>
     * <span data-ttu-id="4a97c-261">Oluşturduğunuz GUID girin **etiketleri** alan.</span><span class="sxs-lookup"><span data-stu-id="4a97c-261">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="4a97c-262">Visual Studio kullanmadığınız durumlarda:</span><span class="sxs-lookup"><span data-stu-id="4a97c-262">When not using Visual Studio:</span></span>
     * <span data-ttu-id="4a97c-263">Uygulamanın proje dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-263">Open the app's project file.</span></span>
     * <span data-ttu-id="4a97c-264">Ekleme bir `<PackageTags>` mevcut veya yeni bir özellik `<PropertyGroup>` oluşturduğunuz GUID'e sahip:</span><span class="sxs-lookup"><span data-stu-id="4a97c-264">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="4a97c-265">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="4a97c-265">In the following example:</span></span>

   * <span data-ttu-id="4a97c-266">Yerel sunucunun IP adresidir `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-266">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="4a97c-267">Çevrimiçi rastgele bir GUID Oluşturucu sağlar `appid` değeri.</span><span class="sxs-lookup"><span data-stu-id="4a97c-267">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="4a97c-268">Bir sertifika kaydedildiğinde, bu aracı ile yanıt veren `SSL Certificate successfully added`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-268">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="4a97c-269">Sertifika kaydı silmek için kullanın `delete sslcert` komutu:</span><span class="sxs-lookup"><span data-stu-id="4a97c-269">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="4a97c-270">Başvuru belgeleri için *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="4a97c-270">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="4a97c-271">Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)</span><span class="sxs-lookup"><span data-stu-id="4a97c-271">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="4a97c-272">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="4a97c-272">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="4a97c-273">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-273">Run the app.</span></span>

   <span data-ttu-id="4a97c-274">Yönetici ayrıcalıkları ile 1024'ten büyük bir bağlantı noktası numarası için localhost HTTP (HTTPS değil) kullanarak bağlanırken, uygulamayı çalıştırmak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-274">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="4a97c-275">Diğer yapılandırmalar için (örneğin, yerel bir IP adresi kullanarak veya bağlama bağlantı noktası 443) uygulamayı yönetici ayrıcalıklarıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4a97c-275">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="4a97c-276">Uygulama sunucusunun genel IP adresinde yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-276">The app responds at the server's public IP address.</span></span> <span data-ttu-id="4a97c-277">Bu örnekte, genel IP adresi, Internet'ten sunucuya ulaştı `104.214.79.47`.</span><span class="sxs-lookup"><span data-stu-id="4a97c-277">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="4a97c-278">Bu örnekte, bir geliştirme sertifikası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4a97c-278">A development certificate is used in this example.</span></span> <span data-ttu-id="4a97c-279">Tarayıcının güvenilmeyen bir sertifika uyarısı atlayarak sonra sayfa güvenli bir şekilde yükler.</span><span class="sxs-lookup"><span data-stu-id="4a97c-279">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![Uygulamanın dizin sayfasını gösteren bir tarayıcı penceresi yüklenmedi](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="4a97c-281">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="4a97c-281">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="4a97c-282">Internet'ten veya kurumsal ağ istekleri etkileşim HTTP.sys tarafından barındırılan uygulamalar için ek yapılandırma Ara sunucuları ve yük dengeleyici barındırırken gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="4a97c-282">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="4a97c-283">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="4a97c-283">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a97c-284">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4a97c-284">Additional resources</span></span>

* [<span data-ttu-id="4a97c-285">HTTP.sys ile Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="4a97c-285">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)
* [<span data-ttu-id="4a97c-286">HTTP Sunucusu API</span><span class="sxs-lookup"><span data-stu-id="4a97c-286">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="4a97c-287">ASP.NET/HttpSysServer GitHub deposu (kaynak kodu)</span><span class="sxs-lookup"><span data-stu-id="4a97c-287">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="4a97c-288">Ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="4a97c-288">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
