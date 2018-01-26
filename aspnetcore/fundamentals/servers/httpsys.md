---
title: "HTTP.sys web server ASP.NET Core uygulamasında"
author: rick-anderson
description: "Bir web sunucusu ASP.NET Core Windows için HTTP.sys tanıtır. Http.Sys çekirdek modu sürücüsü üzerinde oluşturulmuş, HTTP.sys bir IIS olmadan Internet'e doğrudan bağlantı için kullanılan Kestrel alternatifidir."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 6fc356d8ecc1b699269286f244000b493e48a2c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="8c020-104">HTTP.sys web server ASP.NET Core uygulamasında</span><span class="sxs-lookup"><span data-stu-id="8c020-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="8c020-105">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="8c020-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="8c020-106">Bu konuda, yalnızca ASP.NET Core 2.0 ve daha sonra uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8c020-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="8c020-107">ASP.NET Core önceki sürümlerde HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="8c020-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="8c020-108">HTTP.sys olan bir [web sunucusu için ASP.NET Core](index.md) yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="8c020-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="8c020-109">Yerleşik olan [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c020-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="8c020-110">HTTP.sys olan alternatif [Kestrel](kestrel.md) Kestel olmayan bazı özellikler sunar.</span><span class="sxs-lookup"><span data-stu-id="8c020-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="8c020-111">**HTTP.sys IIS veya kullanılamaz IIS Express ile uyumlu olarak [ASP.NET Core Modülü](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="8c020-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="8c020-112">HTTP.sys aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="8c020-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="8c020-113">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="8c020-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="8c020-114">Bağlantı noktası paylaşma</span><span class="sxs-lookup"><span data-stu-id="8c020-114">Port sharing</span></span>
- <span data-ttu-id="8c020-115">SNI ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="8c020-115">HTTPS with SNI</span></span>
- <span data-ttu-id="8c020-116">TLS (Windows 10) üzerinden HTTP/2</span><span class="sxs-lookup"><span data-stu-id="8c020-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="8c020-117">Doğrudan dosya aktarımı</span><span class="sxs-lookup"><span data-stu-id="8c020-117">Direct file transmission</span></span>
- <span data-ttu-id="8c020-118">Yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="8c020-118">Response caching</span></span>
- <span data-ttu-id="8c020-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="8c020-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="8c020-120">Desteklenen Windows sürümlerine:</span><span class="sxs-lookup"><span data-stu-id="8c020-120">Supported Windows versions:</span></span>

- <span data-ttu-id="8c020-121">Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler</span><span class="sxs-lookup"><span data-stu-id="8c020-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="8c020-122">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c020-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="8c020-123">HTTP.sys kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="8c020-123">When to use HTTP.sys</span></span>

<span data-ttu-id="8c020-124">HTTP.sys IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken burada dağıtımları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="8c020-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="8c020-126">Http.Sys inşa edildiğinden, HTTP.sys saldırılara karşı koruma için ters proxy sunucusu olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="8c020-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="8c020-127">Http.Sys pek saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="8c020-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="8c020-128">IIS'nin bir HTTP dinleyicisi Http.Sys üstünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="8c020-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="8c020-129">Windows kimlik doğrulaması gibi Kestrel içinde değil kullanılabilecek bir özellik gerektiğinde HTTP.sys Dahili dağıtımlar için iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="8c020-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys, iç ağınız ile doğrudan iletişim kurar](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="8c020-131">HTTP.sys kullanma</span><span class="sxs-lookup"><span data-stu-id="8c020-131">How to use HTTP.sys</span></span>

<span data-ttu-id="8c020-132">Burada, konak işletim sistemi ve ASP.NET Core uygulamanız için Kurulum görevleri genel bir bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8c020-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="8c020-133">Windows Server yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8c020-133">Configure Windows Server</span></span>

* <span data-ttu-id="8c020-134">Uygulamanızın gerektirdiği, gibi .NET sürümünü yüklemek [.NET Core](https://www.microsoft.com/net/download/core) veya [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="8c020-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="8c020-135">HTTP.sys dosyasına bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister</span><span class="sxs-lookup"><span data-stu-id="8c020-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="8c020-136">URL öneklerini Windows preregister yok, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c020-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="8c020-137">1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlamak tek istisnası; Bu durumda, yönetici ayrıcalıkları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="8c020-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="8c020-138">Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerisinde yer.</span><span class="sxs-lookup"><span data-stu-id="8c020-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="8c020-139">HTTP.sys ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="8c020-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="8c020-140">Kullanabileceğiniz *netsh.exe* veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="8c020-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="8c020-141">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="8c020-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="8c020-142">ASP.NET Core uygulamanızı HTTP.sys kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8c020-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="8c020-143">Paket yükleme kullanırsanız, gerekli [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="8c020-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="8c020-144">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) paketinde metapackage.</span><span class="sxs-lookup"><span data-stu-id="8c020-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="8c020-145">Çağrı `UseHttpSys` genişletme yöntemi `WebHostBuilder` içinde `Main` herhangi belirtme yöntemi [HTTP.sys seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) aşağıdaki örnekte gösterildiği gibi gereken:</span><span class="sxs-lookup"><span data-stu-id="8c020-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="8c020-146">HTTP.sys seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8c020-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="8c020-147">HTTP.sys ayarları ve yapılandırabileceğiniz sınırlarını bazıları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8c020-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="8c020-148">**En fazla istemci bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="8c020-148">**Maximum client connections**</span></span>

<span data-ttu-id="8c020-149">Aşağıdaki kod ile tüm uygulama için en fazla eş zamanlı açık TCP bağlantı sayısını ayarlanabilir *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8c020-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="8c020-150">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null) olur.</span><span class="sxs-lookup"><span data-stu-id="8c020-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="8c020-151">**En büyük istek gövdesi boyutu**</span><span class="sxs-lookup"><span data-stu-id="8c020-151">**Maximum request body size**</span></span>

<span data-ttu-id="8c020-152">Varsayılan en büyük istek gövdesi boyutu 30.000.000, hangi 28.6 MB'dir bayttır.</span><span class="sxs-lookup"><span data-stu-id="8c020-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="8c020-153">Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem kullanmaktır [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) bir eylem yönteminin özniteliği:</span><span class="sxs-lookup"><span data-stu-id="8c020-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="8c020-154">Aşağıda, tüm uygulama, her istek için kısıtlamayı yapılandırmak nasıl oluşturulduğunu gösteren bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8c020-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="8c020-155">Belirli bir istekte ayarı geçersiz kılabilirsiniz *haline*:</span><span class="sxs-lookup"><span data-stu-id="8c020-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="8c020-156">İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırı yapılandırmayı denerseniz özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="8c020-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="8c020-157">Var. bir `IsReadOnly` belirten özelliği, `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="8c020-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="8c020-158">Diğer HTTP.sys seçenekleri hakkında daha fazla bilgi için bkz: [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="8c020-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="8c020-159">Dinlemenin yapılacağı URL ve bağlantı noktalarını yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="8c020-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="8c020-160">Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8c020-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="8c020-161">URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseUrls` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni, ASPNETCORE_URLS ortam değişkeni veya `UrlPrefixes` özelliği [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="8c020-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="8c020-162">Aşağıdaki kod örneğinde `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="8c020-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="8c020-163">Bir avantajı `UrlPrefixes` yanlış biçimlendirilmiş bir önek eklemeyi denediğinizde hata iletisi hemen alırsınız.</span><span class="sxs-lookup"><span data-stu-id="8c020-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="8c020-164">Bir avantajı `UseUrls` (ile paylaşılan `urls` ve ASPNETCORE_URLS), daha kolay Kestrel ve HTTP.sys arasında geçiş yapabilirsiniz emin olan.</span><span class="sxs-lookup"><span data-stu-id="8c020-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="8c020-165">Her ikisi de kullanırsanız, `UseUrls` (veya `urls` veya ASPNETCORE_URLS) ve `UrlPrefixes`, ayarlarında `UrlPrefixes` olanları geçersiz `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="8c020-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="8c020-166">Daha fazla bilgi için bkz: [barındırma](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="8c020-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="8c020-167">HTTP.sys kullanan [HTTP Sunucusu API UrlPrefix dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c020-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="8c020-168">Aynı önek dizelerde belirttiğinizden emin olun `UseUrls` veya `UrlPrefixes` , sunucu üzerinde preregister.</span><span class="sxs-lookup"><span data-stu-id="8c020-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="8c020-169">IIS kullanmayın</span><span class="sxs-lookup"><span data-stu-id="8c020-169">Don't use IIS</span></span>

<span data-ttu-id="8c020-170">Uygulamanızı IIS veya IIS Express çalışmak üzere yapılandırılmamış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8c020-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="8c020-171">Visual Studio'da varsayılan başlatma için IIS Express profilidir.</span><span class="sxs-lookup"><span data-stu-id="8c020-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="8c020-172">Projeyi bir konsol uygulaması olarak çalıştırmak için el ile seçilen profil aşağıdaki ekran görüntüsünde gösterildiği gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8c020-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Konsol uygulama profili seçin](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="8c020-174">URL öneklerini preregister ve SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8c020-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="8c020-175">IIS ve HTTP.sys isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü kullanır ve işleme başlangıç.</span><span class="sxs-lookup"><span data-stu-id="8c020-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="8c020-176">IIS, kullanıcı Arabirimi yönetim her şeyi yapılandırmak için görece olarak daha kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c020-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="8c020-177">Ancak, Http.Sys kendiniz yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c020-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="8c020-178">Bunu yerleşik bir araca 's *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="8c020-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="8c020-179">İle *netsh.exe* yedek URL öneklerini ve SSL sertifikalarını atayın.</span><span class="sxs-lookup"><span data-stu-id="8c020-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="8c020-180">Aracı yönetim ayrıcalıkları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8c020-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="8c020-181">Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için gereken en düşük gösterir:</span><span class="sxs-lookup"><span data-stu-id="8c020-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="8c020-182">Aşağıdaki örnek, bir SSL sertifikası atama gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8c020-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="8c020-183">Başvuru belgelerini işte *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="8c020-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="8c020-184">Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)</span><span class="sxs-lookup"><span data-stu-id="8c020-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="8c020-185">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="8c020-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="8c020-186">Aşağıdaki kaynaklar, çeşitli senaryoları için ayrıntılı yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c020-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="8c020-187">Her ikisi de Http.Sys üzerinde tabanlı olarak HttpListener için başvuran makaleleri HTTP.sys için eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8c020-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="8c020-188">Nasıl Yapılır: SSL Sertifikası ile Bir Bağlantı Noktasını Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8c020-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="8c020-189">[HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf blog ve oldukça eski ancak hala yararlı bilgiler.</span><span class="sxs-lookup"><span data-stu-id="8c020-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="8c020-190">[Nasıl yapılır: Kod (C++) bir SSL basit sunucu olarak izlenecek kullanarak HttpListener veya Http sunucusu yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler ile daha eski bir blog budur.</span><span class="sxs-lookup"><span data-stu-id="8c020-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="8c020-191">Daha kolay olabilir bazı üçüncü taraf araçları işte *netsh.exe* komut satırı.</span><span class="sxs-lookup"><span data-stu-id="8c020-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="8c020-192">Bunlar değil tarafından sağlanan veya Microsoft tarafından onaylanır.</span><span class="sxs-lookup"><span data-stu-id="8c020-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="8c020-193">Varsayılan olarak, yönetici olarak araçları beri çalıştırmak *netsh.exe* kendisi için yönetici ayrıcalıkları gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="8c020-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="8c020-194">[HTTP.sys Yöneticisi](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, önek ayırmaları ve sertifika güven listelerini.</span><span class="sxs-lookup"><span data-stu-id="8c020-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="8c020-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL öneklerini yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c020-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="8c020-196">Kullanıcı arabirimini Yöneticisi http.sys daha gelişmiş ve daha fazla birkaç yapılandırma seçeneği sunar, ancak Aksi halde benzer bir işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c020-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="8c020-197">Yeni bir sertifika güven listesi (CTL) oluşturamaz, ancak var olanları atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c020-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="8c020-198">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="8c020-198">Next steps</span></span>

<span data-ttu-id="8c020-199">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="8c020-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="8c020-200">Bu makale için örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="8c020-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="8c020-201">HTTP.sys kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="8c020-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="8c020-202">Barındırma</span><span class="sxs-lookup"><span data-stu-id="8c020-202">Hosting</span></span>](xref:fundamentals/hosting)
