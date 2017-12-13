---
title: "HTTP.sys web server ASP.NET Core uygulamasında"
author: rick-anderson
description: "Bir web sunucusu ASP.NET Core Windows için HTTP.sys tanıtır. Http.Sys çekirdek modu sürücüsü üzerinde oluşturulmuş, HTTP.sys bir IIS olmadan Internet'e doğrudan bağlantı için kullanılan Kestrel alternatifidir."
keywords: "ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url önekleri, SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d3f9eb4943ed62b674d6bb2ab1b275b0a3c02343
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="3d3a9-105">HTTP.sys web server ASP.NET Core uygulamasında</span><span class="sxs-lookup"><span data-stu-id="3d3a9-105">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="3d3a9-106">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="3d3a9-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="3d3a9-107">Bu konuda, yalnızca ASP.NET Core 2.0 ve daha sonra uygulanır.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-107">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="3d3a9-108">ASP.NET Core önceki sürümlerde HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-108">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="3d3a9-109">HTTP.sys olan bir [web sunucusu için ASP.NET Core](index.md) yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-109">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="3d3a9-110">Yerleşik olan [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="3d3a9-111">HTTP.sys olan alternatif [Kestrel](kestrel.md) Kestel olmayan bazı özellikler sunar.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-111">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="3d3a9-112">**HTTP.sys IIS veya kullanılamaz IIS Express ile uyumlu değil olarak [ASP.NET Core Modülü](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="3d3a9-112">**HTTP.sys can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="3d3a9-113">HTTP.sys aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-113">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="3d3a9-114">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3d3a9-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="3d3a9-115">Bağlantı noktası paylaşma</span><span class="sxs-lookup"><span data-stu-id="3d3a9-115">Port sharing</span></span>
- <span data-ttu-id="3d3a9-116">SNI ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="3d3a9-116">HTTPS with SNI</span></span>
- <span data-ttu-id="3d3a9-117">TLS (Windows 10) üzerinden HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3d3a9-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="3d3a9-118">Doğrudan dosya aktarımı</span><span class="sxs-lookup"><span data-stu-id="3d3a9-118">Direct file transmission</span></span>
- <span data-ttu-id="3d3a9-119">Yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="3d3a9-119">Response caching</span></span>
- <span data-ttu-id="3d3a9-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="3d3a9-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="3d3a9-121">Desteklenen Windows sürümlerine:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-121">Supported Windows versions:</span></span>

- <span data-ttu-id="3d3a9-122">Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler</span><span class="sxs-lookup"><span data-stu-id="3d3a9-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="3d3a9-123">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3d3a9-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="3d3a9-124">HTTP.sys kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="3d3a9-124">When to use HTTP.sys</span></span>

<span data-ttu-id="3d3a9-125">HTTP.sys IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken burada dağıtımları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-125">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="3d3a9-127">Http.Sys inşa edildiğinden, HTTP.sys saldırılara karşı koruma için ters proxy sunucusu olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-127">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="3d3a9-128">Http.Sys pek saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="3d3a9-129">IIS'nin bir HTTP dinleyicisi Http.Sys üstünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="3d3a9-130">Windows kimlik doğrulaması gibi Kestrel içinde değil kullanılabilecek bir özellik gerektiğinde HTTP.sys Dahili dağıtımlar için iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-130">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys, iç ağınız ile doğrudan iletişim kurar](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="3d3a9-132">HTTP.sys kullanma</span><span class="sxs-lookup"><span data-stu-id="3d3a9-132">How to use HTTP.sys</span></span>

<span data-ttu-id="3d3a9-133">Burada, konak işletim sistemi ve ASP.NET Core uygulamanız için Kurulum görevleri genel bir bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="3d3a9-134">Windows Server yapılandırın</span><span class="sxs-lookup"><span data-stu-id="3d3a9-134">Configure Windows Server</span></span>

* <span data-ttu-id="3d3a9-135">Uygulamanızın gerektirdiği, gibi .NET sürümünü yüklemek [.NET Core](https://www.microsoft.com/net/download/core) veya [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-135">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="3d3a9-136">HTTP.sys dosyasına bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister</span><span class="sxs-lookup"><span data-stu-id="3d3a9-136">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="3d3a9-137">URL öneklerini Windows preregister yok, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="3d3a9-138">1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlamak tek istisnası; Bu durumda, yönetici ayrıcalıkları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="3d3a9-139">Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerisinde yer.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="3d3a9-140">HTTP.sys ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-140">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="3d3a9-141">Kullanabileceğiniz *netsh.exe* veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-141">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="3d3a9-142">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="3d3a9-143">ASP.NET Core uygulamanızı HTTP.sys kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3d3a9-143">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="3d3a9-144">Paket yükleme kullanırsanız, gerekli [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-144">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="3d3a9-145">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) paketinde metapackage.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-145">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="3d3a9-146">Çağrı `UseHttpSys` genişletme yöntemi `WebHostBuilder` içinde `Main` herhangi belirtme yöntemi [HTTP.sys seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) aşağıdaki örnekte gösterildiği gibi gereken:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-146">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="3d3a9-147">HTTP.sys seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3d3a9-147">Configure HTTP.sys options</span></span>

<span data-ttu-id="3d3a9-148">HTTP.sys ayarları ve yapılandırabileceğiniz sınırlarını bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-148">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="3d3a9-149">**En fazla istemci bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="3d3a9-149">**Maximum client connections**</span></span>

<span data-ttu-id="3d3a9-150">Aşağıdaki kod ile tüm uygulama için en fazla eş zamanlı açık TCP bağlantı sayısını ayarlanabilir *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-150">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="3d3a9-151">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null) olur.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-151">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="3d3a9-152">**En büyük istek gövdesi boyutu**</span><span class="sxs-lookup"><span data-stu-id="3d3a9-152">**Maximum request body size**</span></span>

<span data-ttu-id="3d3a9-153">Varsayılan en büyük istek gövdesi boyutu 30.000.000, hangi 28.6 MB'dir bayttır.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-153">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="3d3a9-154">Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem kullanmaktır [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) bir eylem yönteminin özniteliği:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-154">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3d3a9-155">Aşağıda, tüm uygulama, her istek için kısıtlamayı yapılandırmak nasıl oluşturulduğunu gösteren bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-155">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="3d3a9-156">Belirli bir istekte ayarı geçersiz kılabilirsiniz *haline*:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-156">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="3d3a9-157">İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırı yapılandırmayı denerseniz özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-157">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="3d3a9-158">Var. bir `IsReadOnly` belirten özelliği, `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-158">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3d3a9-159">Diğer HTTP.sys seçenekleri hakkında daha fazla bilgi için bkz: [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-159">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="3d3a9-160">Dinlemenin yapılacağı URL ve bağlantı noktalarını yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="3d3a9-160">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="3d3a9-161">Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-161">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="3d3a9-162">URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseUrls` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni, ASPNETCORE_URLS ortam değişkeni veya `UrlPrefixes` özelliği [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-162">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="3d3a9-163">Aşağıdaki kod örneğinde `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-163">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="3d3a9-164">Bir avantajı `UrlPrefixes` yanlış biçimlendirilmiş bir önek eklemeyi denediğinizde hata iletisi hemen alırsınız.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-164">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="3d3a9-165">Bir avantajı `UseUrls` (ile paylaşılan `urls` ve ASPNETCORE_URLS), daha kolay Kestrel ve HTTP.sys arasında geçiş yapabilirsiniz emin olan.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-165">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="3d3a9-166">Her ikisi de kullanırsanız, `UseUrls` (veya `urls` veya ASPNETCORE_URLS) ve `UrlPrefixes`, ayarlarında `UrlPrefixes` olanları geçersiz `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-166">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="3d3a9-167">Daha fazla bilgi için bkz: [barındırma](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-167">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="3d3a9-168">HTTP.sys kullanan [HTTP Sunucusu API UrlPrefix dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-168">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="3d3a9-169">Aynı önek dizelerde belirttiğinizden emin olun `UseUrls` veya `UrlPrefixes` , sunucu üzerinde preregister.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-169">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="3d3a9-170">IIS kullanmayın</span><span class="sxs-lookup"><span data-stu-id="3d3a9-170">Don't use IIS</span></span>

<span data-ttu-id="3d3a9-171">Uygulamanızı IIS veya IIS Express çalışmak üzere yapılandırılmamış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-171">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="3d3a9-172">Visual Studio'da varsayılan başlatma için IIS Express profilidir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-172">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="3d3a9-173">Projeyi bir konsol uygulaması olarak çalıştırmak için el ile seçilen profil aşağıdaki ekran görüntüsünde gösterildiği gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-173">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Konsol uygulama profili seçin](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="3d3a9-175">URL öneklerini preregister ve SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3d3a9-175">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="3d3a9-176">IIS ve HTTP.sys isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü kullanır ve işleme başlangıç.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-176">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="3d3a9-177">IIS, kullanıcı Arabirimi yönetim her şeyi yapılandırmak için görece olarak daha kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-177">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="3d3a9-178">Ancak, Http.Sys kendiniz yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-178">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="3d3a9-179">Diğer bir deyişle yapmak için yerleşik aracı *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-179">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="3d3a9-180">İle *netsh.exe* yedek URL öneklerini ve SSL sertifikalarını atayın.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-180">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="3d3a9-181">Aracı yönetim ayrıcalıkları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-181">The tool requires administrative privileges.</span></span>

<span data-ttu-id="3d3a9-182">Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için gereken en düşük gösterir:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-182">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="3d3a9-183">Aşağıdaki örnek, bir SSL sertifikası atama gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-183">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="3d3a9-184">Başvuru belgelerini işte *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-184">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="3d3a9-185">Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)</span><span class="sxs-lookup"><span data-stu-id="3d3a9-185">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="3d3a9-186">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="3d3a9-186">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="3d3a9-187">Aşağıdaki kaynaklar, çeşitli senaryoları için ayrıntılı yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-187">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="3d3a9-188">Her ikisi de Http.Sys üzerinde tabanlı olarak HttpListener için başvuran makaleleri HTTP.sys için eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-188">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="3d3a9-189">Nasıl yapılır: bir SSL sertifikası ile bir bağlantı noktası yapılandırın</span><span class="sxs-lookup"><span data-stu-id="3d3a9-189">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="3d3a9-190">[HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf blog ve oldukça eski ancak hala yararlı bilgiler.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-190">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="3d3a9-191">[Nasıl yapılır: Kod (C++) bir SSL basit sunucu olarak izlenecek kullanarak HttpListener veya Http sunucusu yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler ile daha eski bir blog budur.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-191">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="3d3a9-192">Daha kolay olabilir bazı üçüncü taraf araçları işte *netsh.exe* komut satırı.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-192">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="3d3a9-193">Bunlar değil tarafından sağlanan veya Microsoft tarafından onaylanır.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-193">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="3d3a9-194">Varsayılan olarak, yönetici olarak araçları beri çalıştırmak *netsh.exe* kendisi için yönetici ayrıcalıkları gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-194">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="3d3a9-195">[HTTP.sys Yöneticisi](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, önek ayırmaları ve sertifika güven listelerini.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="3d3a9-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL öneklerini yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="3d3a9-197">Kullanıcı arabirimini Yöneticisi http.sys daha gelişmiş ve daha fazla birkaç yapılandırma seçeneği sunar, ancak Aksi halde benzer bir işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-197">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="3d3a9-198">Yeni bir sertifika güven listesi (CTL) oluşturamaz, ancak var olanları atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-198">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="3d3a9-199">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="3d3a9-199">Next steps</span></span>

<span data-ttu-id="3d3a9-200">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-200">For more information, see the following resources:</span></span>

* [<span data-ttu-id="3d3a9-201">Bu makale için örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="3d3a9-201">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="3d3a9-202">HTTP.sys kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="3d3a9-202">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="3d3a9-203">Barındırma</span><span class="sxs-lookup"><span data-stu-id="3d3a9-203">Hosting</span></span>](xref:fundamentals/hosting)
