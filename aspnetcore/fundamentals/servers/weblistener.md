---
title: ASP.NET Core WebListener web sunucusu uygulaması
author: rick-anderson
description: WebListener, ASP.NET Core IIS olmadan Internet'e doğrudan bağlantı için kullanılan Windows için bir web sunucusu hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: cd2e477824d916afcf1a7901e935dd465a466922
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="c075b-103">ASP.NET Core WebListener web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="c075b-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="c075b-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="c075b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="c075b-105">Bu konu, yalnızca ASP.NET Core geçerlidir 1.x.</span><span class="sxs-lookup"><span data-stu-id="c075b-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="c075b-106">ASP.NET Core 2. 0'WebListener adlı [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="c075b-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="c075b-107">WebListener olan bir [web sunucusu için ASP.NET Core](index.md) yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="c075b-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="c075b-108">Yerleşik olan [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="c075b-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="c075b-109">WebListener olan alternatif [Kestrel](kestrel.md) kullanılabilecek doğrudan Internet bağlantısı için ters proxy sunucusu olarak IIS dayanmayan.</span><span class="sxs-lookup"><span data-stu-id="c075b-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="c075b-110">Aslında, **WebListener IIS veya kullanılamaz IIS Express ile uyumlu değil olarak [ASP.NET Core Modülü](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="c075b-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="c075b-111">WebListener ASP.NET Core için geliştirilen rağmen bunu doğrudan herhangi bir .NET Core veya .NET Framework uygulama kullanılabilir [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="c075b-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="c075b-112">WebListener aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="c075b-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="c075b-113">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="c075b-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="c075b-114">Bağlantı noktası paylaşma</span><span class="sxs-lookup"><span data-stu-id="c075b-114">Port sharing</span></span>
- <span data-ttu-id="c075b-115">SNI ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="c075b-115">HTTPS with SNI</span></span>
- <span data-ttu-id="c075b-116">TLS (Windows 10) üzerinden HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c075b-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="c075b-117">Doğrudan dosya aktarımı</span><span class="sxs-lookup"><span data-stu-id="c075b-117">Direct file transmission</span></span>
- <span data-ttu-id="c075b-118">Yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="c075b-118">Response caching</span></span>
- <span data-ttu-id="c075b-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="c075b-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="c075b-120">Desteklenen Windows sürümlerine:</span><span class="sxs-lookup"><span data-stu-id="c075b-120">Supported Windows versions:</span></span>

- <span data-ttu-id="c075b-121">Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler</span><span class="sxs-lookup"><span data-stu-id="c075b-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="c075b-122">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c075b-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="c075b-123">Ne zaman WebListener kullanılır</span><span class="sxs-lookup"><span data-stu-id="c075b-123">When to use WebListener</span></span>

<span data-ttu-id="c075b-124">WebListener IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken burada dağıtımları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="c075b-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener doğrudan Internet ile iletişim kurar](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="c075b-126">Http.Sys inşa edildiğinden, WebListener saldırılara karşı koruma için ters proxy sunucusu olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="c075b-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="c075b-127">Http.Sys pek saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="c075b-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="c075b-128">IIS'nin bir HTTP dinleyicisi Http.Sys üstünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="c075b-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="c075b-129">Kestrel kullanarak alınamıyor sunduğu özelliklerden birini gerektiğinde WebListener de iyi bir dahili dağıtımlar için seçimdir.</span><span class="sxs-lookup"><span data-stu-id="c075b-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener, iç ağınız ile doğrudan iletişim kurar](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="c075b-131">WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="c075b-131">How to use WebListener</span></span>

<span data-ttu-id="c075b-132">Burada, konak işletim sistemi ve ASP.NET Core uygulamanız için Kurulum görevleri genel bir bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c075b-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="c075b-133">Windows Server yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c075b-133">Configure Windows Server</span></span>

* <span data-ttu-id="c075b-134">Uygulamanızın gerektirdiği, gibi .NET sürümünü yüklemek [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) veya .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="c075b-134">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="c075b-135">WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister</span><span class="sxs-lookup"><span data-stu-id="c075b-135">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="c075b-136">URL öneklerini Windows preregister yok, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c075b-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="c075b-137">1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlamak tek istisnası; Bu durumda yönetici ayrıcalıkları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c075b-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="c075b-138">Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerisinde yer.</span><span class="sxs-lookup"><span data-stu-id="c075b-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="c075b-139">WebListener ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="c075b-139">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="c075b-140">Netsh.exe kullanabilirsiniz veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="c075b-140">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="c075b-141">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="c075b-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="c075b-142">ASP.NET Core uygulamanızı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="c075b-142">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="c075b-143">NuGet paket yüklemesi [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="c075b-143">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="c075b-144">Bu da yükler [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) bağımlılık olarak.</span><span class="sxs-lookup"><span data-stu-id="c075b-144">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="c075b-145">Çağrı `UseWebListener` genişletme yöntemi [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) içinde `Main` yöntemi, hiçbir WebListener belirtme [seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) ve [ayarları](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) gereken , aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="c075b-145">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="c075b-146">Dinlemenin yapılacağı URL ve bağlantı noktalarını yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="c075b-146">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="c075b-147">Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c075b-147">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="c075b-148">URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseURLs` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni veya ASP.NET Core yapılandırma sistemi.</span><span class="sxs-lookup"><span data-stu-id="c075b-148">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="c075b-149">Daha fazla bilgi için bkz: [barındırma](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="c075b-149">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="c075b-150">Dinleyici kullanan web [Http.Sys önek dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="c075b-150">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="c075b-151">WebListener için özel önek dizesi biçimi gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="c075b-151">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="c075b-152">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c075b-152">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="c075b-153">Üst düzey joker bağlamaları uygulamanızı güvenlik açıkları için yedekleme açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c075b-153">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="c075b-154">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c075b-154">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="c075b-155">Joker karakterler yerine açık ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="c075b-155">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="c075b-156">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanı denetlemek, bu güvenlik riskinin yok (tersine `*.com`, açık olduğu).</span><span class="sxs-lookup"><span data-stu-id="c075b-156">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="c075b-157">Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c075b-157">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c075b-158">Aynı önek dizelerde belirttiğinizden emin olun `UseUrls` , sunucu üzerinde preregister.</span><span class="sxs-lookup"><span data-stu-id="c075b-158">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="c075b-159">Uygulamanızı IIS veya IIS Express çalışmak üzere yapılandırılmamış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="c075b-159">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="c075b-160">Visual Studio'da varsayılan başlatma için IIS Express profilidir.</span><span class="sxs-lookup"><span data-stu-id="c075b-160">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="c075b-161">Projeyi bir konsol uygulaması olarak çalıştırmak için el ile seçilen profil değiştirmek için aşağıdaki ekran görüntüsünde gösterildiği gibi olması.</span><span class="sxs-lookup"><span data-stu-id="c075b-161">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Konsol uygulama profili seçin](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="c075b-163">ASP.NET Core dışında WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="c075b-163">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="c075b-164">Yükleme [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="c075b-164">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="c075b-165">[WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister](#preregister-url-prefixes-and-configure-ssl) ASP.NET Core kullanmak için olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="c075b-165">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="c075b-166">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="c075b-166">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="c075b-167">ASP.NET Core dışında WebListener kullanım gösteren bir kod örneği şöyledir:</span><span class="sxs-lookup"><span data-stu-id="c075b-167">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="c075b-168">URL öneklerini preregister ve SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c075b-168">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="c075b-169">IIS ve WebListener isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü kullanır ve işleme başlangıç.</span><span class="sxs-lookup"><span data-stu-id="c075b-169">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="c075b-170">IIS, kullanıcı Arabirimi yönetim her şeyi yapılandırmak için görece olarak daha kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="c075b-170">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="c075b-171">Ancak, WebListener kullanıyorsanız, Http.Sys kendiniz yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c075b-171">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="c075b-172">Netsh.exe olduğundan Bunu yapmak için yerleşik aracı.</span><span class="sxs-lookup"><span data-stu-id="c075b-172">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="c075b-173">İçin netsh.exe kullanmak için gereken en yaygın görevler URL öneklerini ayırma ve SSL sertifikalarını atama.</span><span class="sxs-lookup"><span data-stu-id="c075b-173">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="c075b-174">NetSh.exe yeni başlayanlar için kullanmak için kolay bir aracı değildir.</span><span class="sxs-lookup"><span data-stu-id="c075b-174">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="c075b-175">Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için gereken tam minimum gösterir:</span><span class="sxs-lookup"><span data-stu-id="c075b-175">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="c075b-176">Aşağıdaki örnek, bir SSL sertifikası atama gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c075b-176">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="c075b-177">Resmi başvuru belgeleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c075b-177">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="c075b-178">Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)</span><span class="sxs-lookup"><span data-stu-id="c075b-178">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="c075b-179">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="c075b-179">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="c075b-180">Aşağıdaki kaynaklar, çeşitli senaryoları için ayrıntılı yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="c075b-180">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="c075b-181">Başvurmak makaleleri `HttpListener` için eşit oranda geçerli `WebListener`, her ikisi de Http.Sys üzerinde tabanlı olarak.</span><span class="sxs-lookup"><span data-stu-id="c075b-181">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="c075b-182">Nasıl Yapılır: SSL Sertifikası ile Bir Bağlantı Noktasını Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c075b-182">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="c075b-183">[HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf blog ve oldukça eski ancak hala yararlı bilgiler.</span><span class="sxs-lookup"><span data-stu-id="c075b-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="c075b-184">[Nasıl yapılır: Kod (C++) bir SSL basit sunucu olarak izlenecek kullanarak HttpListener veya Http sunucusu yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler ile daha eski bir blog budur.</span><span class="sxs-lookup"><span data-stu-id="c075b-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="c075b-185">.NET Core WebListener SSL ile nasıl ayarlayabilirim?</span><span class="sxs-lookup"><span data-stu-id="c075b-185">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="c075b-186">Burada, kullanılacak netsh.exe komut satırı kolay olabilir bazı üçüncü taraf araçları bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c075b-186">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="c075b-187">Bunlar değil tarafından sağlanan veya Microsoft tarafından onaylanır.</span><span class="sxs-lookup"><span data-stu-id="c075b-187">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="c075b-188">Netsh.exe kendisine yönetici ayrıcalıkları gerektirdiğinden araçları varsayılan olarak, yönetici olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c075b-188">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="c075b-189">[HTTP.sys Yöneticisi](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, önek ayırmaları ve sertifika güven listelerini.</span><span class="sxs-lookup"><span data-stu-id="c075b-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="c075b-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL öneklerini yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="c075b-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="c075b-191">Kullanıcı arabirimini Yöneticisi http.sys daha gelişmiş ve daha fazla birkaç yapılandırma seçeneği sunar, ancak Aksi halde benzer bir işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="c075b-191">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="c075b-192">Yeni bir sertifika güven listesi (CTL) oluşturamaz, ancak var olanları atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c075b-192">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="c075b-193">Otomatik imzalı SSL sertifikalarını oluşturmak için Microsoft komut satırı araçları sağlar: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) ve PowerShell cmdlet [yeni SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="c075b-193">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="c075b-194">Otomatik olarak imzalanan SSL sertifikalarını oluşturmak kolaylaştırmak üçüncü taraf UI araçları vardır:</span><span class="sxs-lookup"><span data-stu-id="c075b-194">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="c075b-195">SelfCert</span><span class="sxs-lookup"><span data-stu-id="c075b-195">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="c075b-196">MakeCert kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="c075b-196">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="c075b-197">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c075b-197">Next steps</span></span>

<span data-ttu-id="c075b-198">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="c075b-198">For more information, see the following resources:</span></span>

* [<span data-ttu-id="c075b-199">Bu makale için örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="c075b-199">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="c075b-200">WebListener kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="c075b-200">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="c075b-201">Barındırma</span><span class="sxs-lookup"><span data-stu-id="c075b-201">Hosting</span></span>](../hosting.md)
