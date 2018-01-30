---
title: "ASP.NET Core WebListener web sunucusu uygulaması"
author: rick-anderson
description: "WebListener, ASP.NET Core Windows için bir web sunucusu tanıtır. Http.Sys çekirdek modu sürücüsü üzerinde oluşturulan WebListener IIS olmadan Internet'e doğrudan bağlantı için kullanılan Kestrel için alternatiftir."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: fb2e0621645a48f4e603d754d8babbc07a78cae4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="546d9-104">ASP.NET Core WebListener web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="546d9-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="546d9-105">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="546d9-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="546d9-106">Bu konu, yalnızca ASP.NET Core geçerlidir 1.x.</span><span class="sxs-lookup"><span data-stu-id="546d9-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="546d9-107">ASP.NET Core 2. 0'WebListener adlı [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="546d9-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="546d9-108">WebListener olan bir [web sunucusu için ASP.NET Core](index.md) yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="546d9-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="546d9-109">Yerleşik olan [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="546d9-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="546d9-110">WebListener olan alternatif [Kestrel](kestrel.md) kullanılabilecek doğrudan Internet bağlantısı için ters proxy sunucusu olarak IIS dayanmayan.</span><span class="sxs-lookup"><span data-stu-id="546d9-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="546d9-111">Aslında, **WebListener IIS veya kullanılamaz IIS Express ile uyumlu değil olarak [ASP.NET Core Modülü](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="546d9-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="546d9-112">WebListener ASP.NET Core için geliştirilen rağmen bunu doğrudan herhangi bir .NET Core veya .NET Framework uygulama kullanılabilir [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="546d9-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="546d9-113">WebListener aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="546d9-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="546d9-114">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="546d9-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="546d9-115">Bağlantı noktası paylaşma</span><span class="sxs-lookup"><span data-stu-id="546d9-115">Port sharing</span></span>
- <span data-ttu-id="546d9-116">SNI ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="546d9-116">HTTPS with SNI</span></span>
- <span data-ttu-id="546d9-117">TLS (Windows 10) üzerinden HTTP/2</span><span class="sxs-lookup"><span data-stu-id="546d9-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="546d9-118">Doğrudan dosya aktarımı</span><span class="sxs-lookup"><span data-stu-id="546d9-118">Direct file transmission</span></span>
- <span data-ttu-id="546d9-119">Yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="546d9-119">Response caching</span></span>
- <span data-ttu-id="546d9-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="546d9-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="546d9-121">Desteklenen Windows sürümlerine:</span><span class="sxs-lookup"><span data-stu-id="546d9-121">Supported Windows versions:</span></span>

- <span data-ttu-id="546d9-122">Windows 7 ve Windows Server 2008 R2 ve sonraki sürümler</span><span class="sxs-lookup"><span data-stu-id="546d9-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="546d9-123">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="546d9-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="546d9-124">Ne zaman WebListener kullanılır</span><span class="sxs-lookup"><span data-stu-id="546d9-124">When to use WebListener</span></span>

<span data-ttu-id="546d9-125">WebListener IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken burada dağıtımları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="546d9-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener doğrudan Internet ile iletişim kurar](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="546d9-127">Http.Sys inşa edildiğinden, WebListener saldırılara karşı koruma için ters proxy sunucusu olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="546d9-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="546d9-128">Http.Sys pek saldırılarına karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="546d9-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="546d9-129">IIS'nin bir HTTP dinleyicisi Http.Sys üstünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="546d9-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="546d9-130">Kestrel kullanarak alınamıyor sunduğu özelliklerden birini gerektiğinde WebListener de iyi bir dahili dağıtımlar için seçimdir.</span><span class="sxs-lookup"><span data-stu-id="546d9-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener, iç ağınız ile doğrudan iletişim kurar](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="546d9-132">WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="546d9-132">How to use WebListener</span></span>

<span data-ttu-id="546d9-133">Burada, konak işletim sistemi ve ASP.NET Core uygulamanız için Kurulum görevleri genel bir bakış verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="546d9-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="546d9-134">Windows Server yapılandırın</span><span class="sxs-lookup"><span data-stu-id="546d9-134">Configure Windows Server</span></span>

* <span data-ttu-id="546d9-135">Uygulamanızın gerektirdiği, gibi .NET sürümünü yüklemek [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) veya .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="546d9-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="546d9-136">WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister</span><span class="sxs-lookup"><span data-stu-id="546d9-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="546d9-137">URL öneklerini Windows preregister yok, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="546d9-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="546d9-138">1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil) kullanarak Localhost'a bağlamak tek istisnası; Bu durumda yönetici ayrıcalıkları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="546d9-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="546d9-139">Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerisinde yer.</span><span class="sxs-lookup"><span data-stu-id="546d9-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="546d9-140">WebListener ulaşması trafiğine izin veren güvenlik duvarı bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="546d9-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="546d9-141">Netsh.exe kullanabilirsiniz veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="546d9-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="546d9-142">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="546d9-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="546d9-143">ASP.NET Core uygulamanızı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="546d9-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="546d9-144">NuGet paket yüklemesi [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="546d9-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="546d9-145">Bu da yükler [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) bağımlılık olarak.</span><span class="sxs-lookup"><span data-stu-id="546d9-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="546d9-146">Çağrı `UseWebListener` genişletme yöntemi [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) içinde `Main` yöntemi, hiçbir WebListener belirtme [seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) ve [ayarları](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) gereken , aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="546d9-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="546d9-147">Dinlemenin yapılacağı URL ve bağlantı noktalarını yapılandırmak</span><span class="sxs-lookup"><span data-stu-id="546d9-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="546d9-148">Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="546d9-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="546d9-149">URL öneklerini ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseURLs` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni veya ASP.NET Core yapılandırma sistemi.</span><span class="sxs-lookup"><span data-stu-id="546d9-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="546d9-150">Daha fazla bilgi için bkz: [barındırma](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="546d9-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="546d9-151">Dinleyici kullanan web [Http.Sys önek dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="546d9-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="546d9-152">WebListener için özel önek dizesi biçimi gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="546d9-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="546d9-153">Aynı önek dizelerde belirttiğinizden emin olun `UseUrls` , sunucu üzerinde preregister.</span><span class="sxs-lookup"><span data-stu-id="546d9-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="546d9-154">Uygulamanızı IIS veya IIS Express çalışmak üzere yapılandırılmamış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="546d9-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="546d9-155">Visual Studio'da varsayılan başlatma için IIS Express profilidir.</span><span class="sxs-lookup"><span data-stu-id="546d9-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="546d9-156">Projeyi bir konsol uygulaması olarak çalıştırmak için el ile seçilen profil değiştirmek için aşağıdaki ekran görüntüsünde gösterildiği gibi olması.</span><span class="sxs-lookup"><span data-stu-id="546d9-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Konsol uygulama profili seçin](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="546d9-158">ASP.NET Core dışında WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="546d9-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="546d9-159">Yükleme [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="546d9-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="546d9-160">[WebListener için bağlama ve SSL sertifikalarını ayarlamak için URL öneklerini preregister](#preregister-url-prefixes-and-configure-ssl) ASP.NET Core kullanmak için olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="546d9-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="546d9-161">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="546d9-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="546d9-162">ASP.NET Core dışında WebListener kullanım gösteren bir kod örneği şöyledir:</span><span class="sxs-lookup"><span data-stu-id="546d9-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="546d9-163">URL öneklerini preregister ve SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="546d9-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="546d9-164">IIS ve WebListener isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü kullanır ve işleme başlangıç.</span><span class="sxs-lookup"><span data-stu-id="546d9-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="546d9-165">IIS, kullanıcı Arabirimi yönetim her şeyi yapılandırmak için görece olarak daha kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="546d9-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="546d9-166">Ancak, WebListener kullanıyorsanız, Http.Sys kendiniz yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="546d9-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="546d9-167">Netsh.exe olduğundan Bunu yapmak için yerleşik aracı.</span><span class="sxs-lookup"><span data-stu-id="546d9-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="546d9-168">İçin netsh.exe kullanmak için gereken en yaygın görevler URL öneklerini ayırma ve SSL sertifikalarını atama.</span><span class="sxs-lookup"><span data-stu-id="546d9-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="546d9-169">NetSh.exe yeni başlayanlar için kullanmak için kolay bir aracı değildir.</span><span class="sxs-lookup"><span data-stu-id="546d9-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="546d9-170">Aşağıdaki örnek, 80 ve 443 numaralı bağlantı noktaları için URL öneklerini ayırmak için gereken tam minimum gösterir:</span><span class="sxs-lookup"><span data-stu-id="546d9-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="546d9-171">Aşağıdaki örnek, bir SSL sertifikası atama gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="546d9-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="546d9-172">Resmi başvuru belgeleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="546d9-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="546d9-173">Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)</span><span class="sxs-lookup"><span data-stu-id="546d9-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="546d9-174">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="546d9-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="546d9-175">Aşağıdaki kaynaklar, çeşitli senaryoları için ayrıntılı yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="546d9-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="546d9-176">Başvurmak makaleleri `HttpListener` için eşit oranda geçerli `WebListener`, her ikisi de Http.Sys üzerinde tabanlı olarak.</span><span class="sxs-lookup"><span data-stu-id="546d9-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="546d9-177">Nasıl Yapılır: SSL Sertifikası ile Bir Bağlantı Noktasını Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="546d9-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="546d9-178">[HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf blog ve oldukça eski ancak hala yararlı bilgiler.</span><span class="sxs-lookup"><span data-stu-id="546d9-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="546d9-179">[Nasıl yapılır: Kod (C++) bir SSL basit sunucu olarak izlenecek kullanarak HttpListener veya Http sunucusu yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler ile daha eski bir blog budur.</span><span class="sxs-lookup"><span data-stu-id="546d9-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="546d9-180">.NET Core WebListener SSL ile nasıl ayarlayabilirim?</span><span class="sxs-lookup"><span data-stu-id="546d9-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="546d9-181">Burada, kullanılacak netsh.exe komut satırı kolay olabilir bazı üçüncü taraf araçları bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="546d9-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="546d9-182">Bunlar değil tarafından sağlanan veya Microsoft tarafından onaylanır.</span><span class="sxs-lookup"><span data-stu-id="546d9-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="546d9-183">Netsh.exe kendisine yönetici ayrıcalıkları gerektirdiğinden araçları varsayılan olarak, yönetici olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="546d9-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="546d9-184">[HTTP.sys Yöneticisi](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, önek ayırmaları ve sertifika güven listelerini.</span><span class="sxs-lookup"><span data-stu-id="546d9-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="546d9-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL öneklerini yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="546d9-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="546d9-186">Kullanıcı arabirimini Yöneticisi http.sys daha gelişmiş ve daha fazla birkaç yapılandırma seçeneği sunar, ancak Aksi halde benzer bir işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="546d9-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="546d9-187">Yeni bir sertifika güven listesi (CTL) oluşturamaz, ancak var olanları atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="546d9-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="546d9-188">Otomatik imzalı SSL sertifikalarını oluşturmak için Microsoft komut satırı araçları sağlar: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) ve PowerShell cmdlet [yeni SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="546d9-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="546d9-189">Otomatik olarak imzalanan SSL sertifikalarını oluşturmak kolaylaştırmak üçüncü taraf UI araçları vardır:</span><span class="sxs-lookup"><span data-stu-id="546d9-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="546d9-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="546d9-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="546d9-191">MakeCert kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="546d9-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="546d9-192">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="546d9-192">Next steps</span></span>

<span data-ttu-id="546d9-193">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="546d9-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="546d9-194">Bu makale için örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="546d9-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="546d9-195">WebListener kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="546d9-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="546d9-196">Barındırma</span><span class="sxs-lookup"><span data-stu-id="546d9-196">Hosting</span></span>](../hosting.md)
