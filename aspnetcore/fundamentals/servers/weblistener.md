---
title: ASP.NET core'da WebListener web sunucusu uygulaması
author: rick-anderson
description: WebListener, bir web sunucusu IIS olmadan İnternet'e doğrudan bağlantı için kullanılan Windows üzerinde ASP.NET Core hakkında bilgi edinin.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5602c1ddbe76879587de12bcd82722c103dee03f
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756362"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="9e6c7-103">ASP.NET core'da WebListener web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="9e6c7-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="9e6c7-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="9e6c7-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="9e6c7-105">Bu konu, yalnızca ASP.NET Core için geçerlidir. 1.x.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="9e6c7-106">ASP.NET Core 2.0 sürümünde WebListener adlı [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="9e6c7-107">WebListener olduğu bir [ASP.NET Core web sunucusu](index.md) yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="9e6c7-108">Yerleşik [Http.Sys çekirdek modu sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="9e6c7-109">WebListener olan alternatif [Kestrel](kestrel.md) kullanılabilecek doğrudan Internet bağlantısı için ters proxy sunucusu olarak IIS üzerinde bağlı olmadan.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="9e6c7-110">Aslında, **WebListener kullanılamaz IIS veya IIS Express ile uyumlu değil olarak [ASP.NET Core Modülü](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="9e6c7-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="9e6c7-111">WebListener ASP.NET Core için geliştirilmiş olsa da, bunu doğrudan herhangi bir .NET Core veya .NET Framework uygulama kullanılabilir [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="9e6c7-112">WebListener aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="9e6c7-113">Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="9e6c7-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="9e6c7-114">Bağlantı noktası paylaşma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-114">Port sharing</span></span>
- <span data-ttu-id="9e6c7-115">SNI ile HTTPS</span><span class="sxs-lookup"><span data-stu-id="9e6c7-115">HTTPS with SNI</span></span>
- <span data-ttu-id="9e6c7-116">HTTP/2 üzerinden TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="9e6c7-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="9e6c7-117">Doğrudan bir dosya aktarımı</span><span class="sxs-lookup"><span data-stu-id="9e6c7-117">Direct file transmission</span></span>
- <span data-ttu-id="9e6c7-118">Yanıtları önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-118">Response caching</span></span>
- <span data-ttu-id="9e6c7-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="9e6c7-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="9e6c7-120">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-120">Supported Windows versions:</span></span>

- <span data-ttu-id="9e6c7-121">Windows 7 ve Windows Server 2008 R2 ve üzeri</span><span class="sxs-lookup"><span data-stu-id="9e6c7-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="9e6c7-122">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e6c7-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="9e6c7-123">WebListener kullanıldığı durumlar</span><span class="sxs-lookup"><span data-stu-id="9e6c7-123">When to use WebListener</span></span>

<span data-ttu-id="9e6c7-124">WebListener, IIS kullanmadan sunucunun İnternete doğrudan kullanıma sunmak gereken dağıtımları için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener doğrudan Internet ile iletişim kurar.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="9e6c7-126">Http.Sys üzerinde oluşturulduğundan, WebListener saldırılara karşı koruma için ters Ara sunucu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="9e6c7-127">Http.Sys, pek çok saldırılara karşı korur ve sağlamlık, güvenlik ve tam özellikli bir web sunucusu ölçeklenebilirliğini sağlayan olgun teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="9e6c7-128">IIS'nin, Http.Sys üzerine bir HTTP dinleyicisi olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span>

<span data-ttu-id="9e6c7-129">Kestrel'i kullanarak alınamıyor sunduğu özelliklerden biri, gereksinim duyduğunuz WebListener de iç dağıtımları için iyi bir seçimdir andır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener doğrudan iç ağınıza ile iletişim kurar.](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="9e6c7-131">Çekirdek modu kimlik doğrulamasını Kerberos ile</span><span class="sxs-lookup"><span data-stu-id="9e6c7-131">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="9e6c7-132">Çekirdek modu kimlik doğrulaması Kerberos kimlik doğrulama protokolü WebListener temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-132">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="9e6c7-133">Kullanıcı modu kimlik doğrulaması, Kerberos ve WebListener ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-133">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="9e6c7-134">Makine hesabı Kerberos belirteci/Active Directory'den elde edilen anahtar şifresini çözmek için kullanılan ve kullanıcının kimliğini doğrulamak için istemcinin sunucuya iletilir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-134">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="9e6c7-135">Hizmet asıl adı (SPN) konak için değil uygulamanın kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-135">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-weblistener"></a><span data-ttu-id="9e6c7-136">WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-136">How to use WebListener</span></span>

<span data-ttu-id="9e6c7-137">Kurulum görevleri için ana işletim sistemi ve ASP.NET Core uygulamanızı genel bir bakış aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-137">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="9e6c7-138">Windows Server'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-138">Configure Windows Server</span></span>

* <span data-ttu-id="9e6c7-139">Uygulamanızın gerektirdiği, gibi .NET sürümünü [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) veya .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-139">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="9e6c7-140">WebListener için bağlama ve SSL sertifikaları ayarlama URL ön ekleri preregister</span><span class="sxs-lookup"><span data-stu-id="9e6c7-140">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="9e6c7-141">Windows URL ön ekleri preregister yoksa, uygulamanızın yönetici ayrıcalıklarıyla çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-141">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="9e6c7-142">1024'ten büyük bir bağlantı noktası numarası ile HTTP (HTTPS değil)'ı kullanarak Localhost'a bağlama tek istisnası; Bu durumda yönetici ayrıcalıkları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-142">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="9e6c7-143">Ayrıntılar için bkz [önekleri preregister ve SSL yapılandırma](#preregister-url-prefixes-and-configure-ssl) bu makalenin ilerleyen bölümlerinde.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-143">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="9e6c7-144">Trafiğin WebListener ulaşmasına izin vermek için güvenlik duvarı bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-144">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="9e6c7-145">Netsh.exe kullanabilirsiniz veya [PowerShell cmdlet'leri](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-145">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="9e6c7-146">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-146">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="9e6c7-147">ASP.NET Core uygulamanızı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-147">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="9e6c7-148">NuGet paketini yüklemek [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-148">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="9e6c7-149">Bu da yükler [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) bağımlılık olarak.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-149">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="9e6c7-150">Çağrı `UseWebListener` genişletme yöntemini [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) içinde `Main` yöntemi, tüm WebListener belirtme [seçenekleri](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) ve [ayarları](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) gereken , aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-150">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="9e6c7-151">URL ve bağlantı noktası dinleyecek şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-151">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="9e6c7-152">Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-152">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9e6c7-153">URL ön ekleri ve bağlantı noktalarını yapılandırmak için kullanabileceğiniz `UseURLs` genişletme yöntemi `urls` komut satırı bağımsız değişkeni veya ASP.NET Core yapılandırma sistemi.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-153">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="9e6c7-154">Daha fazla bilgi için bkz. [ASP.NET Core(xref:fundamentals/host/index) ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-154">For more information, see [Host in ASP.NET Core(xref:fundamentals/host/index).</span></span>

  <span data-ttu-id="9e6c7-155">Dinleyici kullanan web [Http.Sys önek dize biçimleri](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-155">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="9e6c7-156">WebListener için özel ön eki dizesi biçimi gereksinimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-156">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="9e6c7-157">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-157">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="9e6c7-158">Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-158">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="9e6c7-159">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-159">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="9e6c7-160">Joker karakterler yerine açık bir ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-160">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="9e6c7-161">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-161">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="9e6c7-162">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="9e6c7-163">Aynı ön eki dizelerde belirttiğinizden emin olun `UseUrls` , sunucuda preregister.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-163">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="9e6c7-164">Uygulamanızı IIS veya IIS Express çalıştırmak için yapılandırılmamış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-164">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="9e6c7-165">Visual Studio'da varsayılan başlatma için IIS Express profilidir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-165">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="9e6c7-166">Projeyi konsol uygulaması olarak çalıştırmak için el ile seçilen profil değiştirmek aşağıdaki ekran görüntüsünde gösterildiği gibi olması.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-166">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Konsol uygulama profili seçin](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="9e6c7-168">ASP.NET Core dışında WebListener kullanma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-168">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="9e6c7-169">Yükleme [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-169">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="9e6c7-170">[WebListener için bağlama ve SSL sertifikaları ayarlama URL ön ekleri preregister](#preregister-url-prefixes-and-configure-ssl) kullanılmak üzere ASP.NET Core gibi.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-170">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="9e6c7-171">Ayrıca [Http.Sys kayıt defteri ayarları](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-171">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="9e6c7-172">ASP.NET Core dışında WebListener kullanım gösteren kod örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-172">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="9e6c7-173">URL ön ekleri preregister ve SSL'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-173">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="9e6c7-174">Hem IIS hem de WebListener isteklerini dinlemek için temel alınan Http.Sys çekirdek modu sürücüsü özelliğine dayanır ve işleme ilk.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-174">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="9e6c7-175">IIS, yönetimi kullanıcı Arabirimi, her şeyi yapılandırmak için daha kolay bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-175">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="9e6c7-176">Ancak, WebListener kullanıyorsanız, Http.Sys kendiniz yapılandırmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-176">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="9e6c7-177">Netsh.exe, bunu yapmak için yerleşik aracı.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-177">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="9e6c7-178">En yaygın görevleri için netsh.exe kullanmanız gereken URL ön ekleri ayırma ve SSL sertifikaları atama.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-178">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="9e6c7-179">NetSh.exe, yeni başlayanlar için kolay bir aracı değildir.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-179">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="9e6c7-180">Aşağıdaki örnek, 80 ve 443 bağlantı noktaları için URL ön ekleri ayırmada gereken en az gösterir:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-180">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="9e6c7-181">Aşağıdaki örnek, bir SSL sertifikası atamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-181">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="9e6c7-182">Resmi başvuru belgeleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-182">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="9e6c7-183">Köprü metni için Netsh komutları Aktarım Protokolü (HTTP)</span><span class="sxs-lookup"><span data-stu-id="9e6c7-183">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="9e6c7-184">UrlPrefix dizeleri</span><span class="sxs-lookup"><span data-stu-id="9e6c7-184">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="9e6c7-185">Aşağıdaki kaynaklar, çeşitli senaryolar için ayrıntılı yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-185">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="9e6c7-186">Başvuran makaleler `HttpListener` için eşit oranda geçerli `WebListener`gibi her ikisi de Http.Sys üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-186">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="9e6c7-187">Nasıl Yapılır: SSL Sertifikası ile Bir Bağlantı Noktasını Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-187">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="9e6c7-188">[HTTPS iletişimi - HttpListener tabanlı barındırma ve istemci sertifikası](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) bu üçüncü taraf Web günlüğü ve oldukça eskidir ancak yine de yararlı bilgiler vardır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-188">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="9e6c7-189">[Nasıl yapılır: Kılavuz kullanarak HttpListener veya Http sunucusu SSL basit sunucu olarak kod (C++) yönetilmeyen](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) çok yararlı bilgiler içeren eski bir blog budur.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="9e6c7-190">SSL ile bir .NET Core WebListener nasıl ayarlayabilirim?</span><span class="sxs-lookup"><span data-stu-id="9e6c7-190">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="9e6c7-191">Kullanılacak netsh.exe komut satırı kolay olabilir bazı üçüncü taraf araçları şunlardır.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-191">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="9e6c7-192">Bunlar değil tarafından sağlanan veya Microsoft tarafından desteklendiğini düşündürecek.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="9e6c7-193">Netsh.exe kendisine yönetici ayrıcalıkları gerektirdiğinden araçları varsayılan olarak, yönetici olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-193">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="9e6c7-194">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/) listesi için kullanıcı Arabirimi sağlar ve SSL sertifikaları ve seçenekleri yapılandırmak, ayırmaları ön ek ve sertifika güven listeleri.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="9e6c7-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) listesinde ya da SSL sertifikaları ve URL ön ekleri yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="9e6c7-196">Kullanıcı arabirimini Manager http.sys daha daraltılmış ve birkaç daha fazla yapılandırma seçeneği sunar, ancak Aksi takdirde, benzer bir işlevsellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="9e6c7-197">Yeni bir sertifika güven listesi (CTL) oluşturulamıyor, ancak var olanları atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e6c7-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="9e6c7-198">Otomatik olarak imzalanan SSL sertifikaları oluşturmak için Microsoft komut satırı araçları sağlar: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) ve PowerShell cmdlet'ini [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="9e6c7-198">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="9e6c7-199">Ayrıca üçüncü taraf kullanıcı Arabirimi otomatik olarak imzalanan SSL sertifikalarını oluşturmak kolaylaştıran bir araç da vardır:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-199">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="9e6c7-200">SelfCert</span><span class="sxs-lookup"><span data-stu-id="9e6c7-200">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="9e6c7-201">MakeCert kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="9e6c7-201">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="9e6c7-202">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="9e6c7-202">Next steps</span></span>

<span data-ttu-id="9e6c7-203">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="9e6c7-203">For more information, see the following resources:</span></span>

* [<span data-ttu-id="9e6c7-204">Bu makalede örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="9e6c7-204">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="9e6c7-205">WebListener kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="9e6c7-205">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="9e6c7-206">Barındırma</span><span class="sxs-lookup"><span data-stu-id="9e6c7-206">Hosting</span></span>](xref:fundamentals/host/index)
