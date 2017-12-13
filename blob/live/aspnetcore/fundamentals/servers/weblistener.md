---
title: "ASP.NET Core WebListener web sunucusu uygulaması"
author: rick-anderson
description: "WebListener, ASP.NET Core Windows için bir web sunucusu tanıtır. Http.Sys çekirdek modu sürücüsü üzerinde oluşturulan WebListener IIS olmadan Internet'e doğrudan bağlantı için kullanılan Kestrel için alternatiftir."
keywords: "ASP.NET Core, WebListener, HttpListener, url öneklerini, SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1abb3558546cd907c78b44d9353d9c9f1f5aff1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="a67b9-105">ASP.NET Core WebListener web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="a67b9-105">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="a67b9-106">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="a67b9-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="a67b9-107">Bu konu, yalnızca ASP.NET Core geçerlidir 1.x.</span><span class="sxs-lookup"><span data-stu-id="a67b9-107">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="a67b9-108">ASP.NET Core 2. 0'WebListener adlı [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="a67b9-108">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="a67b9-109">WebListener olan bir [web sunucusu için ASP.NET Core](index.md) yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="a67b9-109">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="a67b9-110">Yerleşik olan [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="a67b9-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="a67b9-111">WebListener olan alternatif [Kestrel](kestrel.md) kullanılabilecek doğrudan Internet bağlantısı için ters proxy sunucusu olarak IIS dayanmayan.</span><span class="sxs-lookup"><span data-stu-id="a67b9-111">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="a67b9-112">Aslında, **WebListener IIS veya kullanılamaz IIS Express ile uyumlu değil olarak [ASP.NET Core Modülü](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="a67b9-112">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="a67b9-113">WebListener ASP.NET Core için geliştirilen rağmen bunu doğrudan herhangi bir .NET Core veya .NET Framework uygulama kullanılabilir [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="a67b9-113">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="a67b9-114">WebListener aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="a67b9-114">WebListener supports the following features:</span></span>

- [<span data-ttu-id="a67b9-115">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="a67b9-115">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="a67b9-116">Bağlantı noktası paylaşma</span><span class="sxs-lookup"><span data-stu-id="a67b9-116">Port sharing</span></span>
- <span data-ttu-id="a67b9-117">SNI ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="a67b9-117">HTTPS with SNI</span></span>
- <span data-ttu-id="a67b9-118">TLS (Windows 10) üzerinden HTTP/2</span><span class="sxs-lookup"><span data-stu-id="a67b9-118">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="a67b9-119">Doğrudan dosya aktarımı</span><span class="sxs-lookup"><span data-stu-id="a67b9-119">Direct file transmission</span></span>
- <span data-ttu-id="a67b9-120">Yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="a67b9-120">Response caching</span></span>
- <span data-ttu-id="a67b9-121">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="a67b9-121">WebSockets (Windows 8)</span></span>

<span data-ttu-id="a67b9-122">Desteklenen Windows sürümlerine:</span><span class="sxs-lookup"><span data-stu-id="a67b9-122">Supported Windows versions:</span></span>

- <span data-ttu-id="a67b9-123">Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler</span><span class="sxs-lookup"><span data-stu-id="a67b9-123">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="a67b9-124">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a67b9-124">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="a67b9-125">Ne zaman WebListener kullanılır</span><span class="sxs-lookup"><span data-stu-id="a67b9-125">When to use WebListener</span></span>

<span data-ttu-id="a67b9-126">WebListener IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken burada dağıtımları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a67b9-126">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener doğrudan Internet ile iletişim kurar](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="a67b9-128">Http.Sys inşa edildiğinden, WebListener saldırılara karşı koruma için ters proxy sunucusu olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="a67b9-128">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="a67b9-129">Http.Sys pek saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-129">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="a67b9-130">IIS'nin bir HTTP dinleyicisi Http.Sys üstünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="a67b9-130">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="a67b9-131">Kestrel kullanarak alınamıyor sunduğu özelliklerden birini gerektiğinde WebListener de iyi bir dahili dağıtımlar için seçimdir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-131">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener, iç ağınız ile doğrudan iletişim kurar](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="a67b9-133">WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="a67b9-133">How to use WebListener</span></span>

<span data-ttu-id="a67b9-134">Burada, konak işletim sistemi ve ASP.NET Core uygulamanız için Kurulum görevleri genel bir bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-134">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="a67b9-135">Windows Server yapılandırın</span><span class="sxs-lookup"><span data-stu-id="a67b9-135">Configure Windows Server</span></span>

* <span data-ttu-id="a67b9-136">Uygulamanızın gerektirdiği, gibi .NET sürümünü yüklemek [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) veya .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="a67b9-136">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="a67b9-137">WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister</span><span class="sxs-lookup"><span data-stu-id="a67b9-137">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="a67b9-138">URL öneklerini Windows preregister yok, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-138">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="a67b9-139">1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlamak tek istisnası; Bu durumda yönetici ayrıcalıkları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-139">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="a67b9-140">Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerisinde yer.</span><span class="sxs-lookup"><span data-stu-id="a67b9-140">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="a67b9-141">WebListener ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="a67b9-141">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="a67b9-142">Netsh.exe kullanabilirsiniz veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="a67b9-142">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="a67b9-143">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="a67b9-143">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="a67b9-144">ASP.NET Core uygulamanızı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="a67b9-144">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="a67b9-145">NuGet paket yüklemesi [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="a67b9-145">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="a67b9-146">Bu da yükler [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) bağımlılık olarak.</span><span class="sxs-lookup"><span data-stu-id="a67b9-146">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="a67b9-147">Çağrı `UseWebListener` genişletme yöntemi [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) içinde `Main` yöntemi, hiçbir WebListener belirtme [seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) ve [ayarları](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) gereken , aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="a67b9-147">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="a67b9-148">Dinlemenin yapılacağı URL ve bağlantı noktalarını yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="a67b9-148">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="a67b9-149">Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a67b9-149">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="a67b9-150">URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseURLs` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni veya ASP.NET Core yapılandırma sistemi.</span><span class="sxs-lookup"><span data-stu-id="a67b9-150">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="a67b9-151">Daha fazla bilgi için bkz: [barındırma](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="a67b9-151">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="a67b9-152">Dinleyici kullanan web [Http.Sys önek dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="a67b9-152">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="a67b9-153">WebListener için özel önek dizesi biçimi gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="a67b9-153">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="a67b9-154">Aynı önek dizelerde belirttiğinizden emin olun `UseUrls` , sunucu üzerinde preregister.</span><span class="sxs-lookup"><span data-stu-id="a67b9-154">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="a67b9-155">Uygulamanızı IIS veya IIS Express çalışmak üzere yapılandırılmamış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="a67b9-155">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="a67b9-156">Visual Studio'da varsayılan başlatma için IIS Express profilidir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-156">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="a67b9-157">Projeyi bir konsol uygulaması olarak çalıştırmak için el ile seçilen profil değiştirmek için aşağıdaki ekran görüntüsünde gösterildiği gibi olması.</span><span class="sxs-lookup"><span data-stu-id="a67b9-157">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Konsol uygulama profili seçin](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="a67b9-159">ASP.NET Core dışında WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="a67b9-159">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="a67b9-160">Yükleme [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="a67b9-160">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="a67b9-161">[WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister](#preregister-url-prefixes-and-configure-ssl) ASP.NET Core kullanmak için olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="a67b9-161">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="a67b9-162">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="a67b9-162">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="a67b9-163">ASP.NET Core dışında WebListener kullanım gösteren bir kod örneği şöyledir:</span><span class="sxs-lookup"><span data-stu-id="a67b9-163">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="a67b9-164">URL öneklerini preregister ve SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a67b9-164">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="a67b9-165">IIS ve WebListener isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü kullanır ve işleme başlangıç.</span><span class="sxs-lookup"><span data-stu-id="a67b9-165">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="a67b9-166">IIS, kullanıcı Arabirimi yönetim her şeyi yapılandırmak için görece olarak daha kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="a67b9-166">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="a67b9-167">Ancak, WebListener kullanıyorsanız, Http.Sys kendiniz yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-167">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="a67b9-168">Netsh.exe, bunu için yerleşik aracıdır.</span><span class="sxs-lookup"><span data-stu-id="a67b9-168">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="a67b9-169">İçin netsh.exe kullanmak için gereken en yaygın görevler URL öneklerini ayırma ve SSL sertifikalarını atama.</span><span class="sxs-lookup"><span data-stu-id="a67b9-169">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="a67b9-170">NetSh.exe yeni başlayanlar için kullanmak için kolay bir aracı değildir.</span><span class="sxs-lookup"><span data-stu-id="a67b9-170">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="a67b9-171">Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için gereken tam minimum gösterir:</span><span class="sxs-lookup"><span data-stu-id="a67b9-171">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="a67b9-172">Aşağıdaki örnek, bir SSL sertifikası atama gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a67b9-172">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="a67b9-173">Resmi başvuru belgeleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a67b9-173">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="a67b9-174">Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)</span><span class="sxs-lookup"><span data-stu-id="a67b9-174">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="a67b9-175">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="a67b9-175">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="a67b9-176">Aşağıdaki kaynaklar, çeşitli senaryoları için ayrıntılı yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="a67b9-176">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="a67b9-177">Başvurmak makaleleri `HttpListener` için eşit oranda geçerli `WebListener`, her ikisi de Http.Sys üzerinde tabanlı olarak.</span><span class="sxs-lookup"><span data-stu-id="a67b9-177">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="a67b9-178">Nasıl yapılır: bir SSL sertifikası ile bir bağlantı noktası yapılandırın</span><span class="sxs-lookup"><span data-stu-id="a67b9-178">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="a67b9-179">[HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf blog ve oldukça eski ancak hala yararlı bilgiler.</span><span class="sxs-lookup"><span data-stu-id="a67b9-179">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="a67b9-180">[Nasıl yapılır: Kod (C++) bir SSL basit sunucu olarak izlenecek kullanarak HttpListener veya Http sunucusu yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler ile daha eski bir blog budur.</span><span class="sxs-lookup"><span data-stu-id="a67b9-180">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="a67b9-181">.NET Core WebListener SSL ile nasıl ayarlayabilirim?</span><span class="sxs-lookup"><span data-stu-id="a67b9-181">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="a67b9-182">Burada, kullanılacak netsh.exe komut satırı kolay olabilir bazı üçüncü taraf araçları bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a67b9-182">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="a67b9-183">Bunlar değil tarafından sağlanan veya Microsoft tarafından onaylanır.</span><span class="sxs-lookup"><span data-stu-id="a67b9-183">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="a67b9-184">Netsh.exe kendisine yönetici ayrıcalıkları gerektirdiğinden araçları varsayılan olarak, yönetici olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a67b9-184">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="a67b9-185">[HTTP.sys Yöneticisi](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, önek ayırmaları ve sertifika güven listelerini.</span><span class="sxs-lookup"><span data-stu-id="a67b9-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="a67b9-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL öneklerini yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="a67b9-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="a67b9-187">Kullanıcı arabirimini Yöneticisi http.sys daha gelişmiş ve daha fazla birkaç yapılandırma seçeneği sunar, ancak Aksi halde benzer bir işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="a67b9-187">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="a67b9-188">Yeni bir sertifika güven listesi (CTL) oluşturamaz, ancak var olanları atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a67b9-188">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="a67b9-189">Otomatik imzalı SSL sertifikalarını oluşturmak için Microsoft komut satırı araçları sağlar: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) ve PowerShell cmdlet [yeni SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="a67b9-189">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="a67b9-190">Otomatik olarak imzalanan SSL sertifikalarını oluşturmak kolaylaştırmak üçüncü taraf UI araçları vardır:</span><span class="sxs-lookup"><span data-stu-id="a67b9-190">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="a67b9-191">SelfCert</span><span class="sxs-lookup"><span data-stu-id="a67b9-191">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="a67b9-192">MakeCert kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="a67b9-192">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="a67b9-193">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="a67b9-193">Next steps</span></span>

<span data-ttu-id="a67b9-194">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="a67b9-194">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a67b9-195">Bu makale için örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="a67b9-195">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="a67b9-196">WebListener kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="a67b9-196">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="a67b9-197">Barındırma</span><span class="sxs-lookup"><span data-stu-id="a67b9-197">Hosting</span></span>](../hosting.md)
