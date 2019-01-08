---
title: ASP.NET Core Nginx ile Linux'ta barındırma
author: rick-anderson
description: Ngınx Kestrel üzerinde çalışan ASP.NET Core web uygulaması HTTP trafiği iletmek için Ubuntu 16.04 ters bir proxy olarak ayarlamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/20/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 24973e7bedcb219ac411948db8aa27d7219eac31
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099292"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="13209-103">ASP.NET Core Nginx ile Linux'ta barındırma</span><span class="sxs-lookup"><span data-stu-id="13209-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="13209-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="13209-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="13209-105">Bu kılavuzda bir Ubuntu 16.04 sunucusunda bir üretime hazır ASP.NET Core ortamını ayarlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="13209-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="13209-106">Büyük olasılıkla bu yönergeleri Ubuntu daha yeni sürümleri ile çalışır, ancak yeni sürümleriyle yönergeleri test edilmemiştir.</span><span class="sxs-lookup"><span data-stu-id="13209-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="13209-107">ASP.NET Core tarafından desteklenen diğer Linux dağıtımları hakkında daha fazla bilgi için bkz: [Linux üzerinde .NET Core önkoşulları](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="13209-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="13209-108">Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="13209-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="13209-109">*systemd* Ubuntu 14.04 üzerinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="13209-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="13209-110">Ubuntu 14.04 yönergeler için bkz: [bu konunun önceki sürümü](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="13209-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="13209-111">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="13209-111">This guide:</span></span>

* <span data-ttu-id="13209-112">Mevcut bir ASP.NET Core uygulaması bir ters proxy sunucusunun arkasında yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="13209-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="13209-113">Ters proxy sunucuyu Kestrel web sunucusu istekleri iletmek için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13209-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="13209-114">Web uygulaması başlangıç bir daemon olarak gerçekleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="13209-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="13209-115">Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="13209-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13209-116">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="13209-116">Prerequisites</span></span>

1. <span data-ttu-id="13209-117">Bir Ubuntu 16.04 sunucusuyla sudo ayrıcalıklarıyla standart kullanıcı hesabı erişimi.</span><span class="sxs-lookup"><span data-stu-id="13209-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="13209-118">.NET Core çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="13209-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="13209-119">Ziyaret [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="13209-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="13209-120">En son Önizleme çalışma zamanı altında listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="13209-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="13209-121">Seçin ve Ubuntu için Ubuntu server sürümüyle eşleşen yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="13209-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="13209-122">Mevcut bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="13209-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="13209-123">Yayımlama ve uygulamanın üzerine kopyalayın</span><span class="sxs-lookup"><span data-stu-id="13209-123">Publish and copy over the app</span></span>

<span data-ttu-id="13209-124">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="13209-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="13209-125">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulamayı paketlemek için geliştirme ortamından (örneğin, *bin/yayın/&lt;target_framework_moniker&gt;/ publish*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="13209-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="13209-126">Uygulama ayrıca olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) .NET Core çalışma zamanı sunucuda değil sağlamak isterseniz.</span><span class="sxs-lookup"><span data-stu-id="13209-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="13209-127">ASP.NET Core uygulaması (örneğin, SCP, SFTP) kuruluşun iş akışınıza tümleştirir bir aracını kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="13209-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="13209-128">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *www/var/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="13209-128">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="13209-129">Üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="13209-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="13209-130">Uygulamayı test edin:</span><span class="sxs-lookup"><span data-stu-id="13209-130">Test the app:</span></span>

1. <span data-ttu-id="13209-131">Uygulamayı komut satırından çalıştırın: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="13209-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="13209-132">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulamayı Linux üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13209-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="13209-133">Ters proxy sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="13209-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="13209-134">Ters proxy hizmet dinamik web uygulamaları için ortak bir kurulum var.</span><span class="sxs-lookup"><span data-stu-id="13209-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="13209-135">Ters proxy, HTTP isteği sonlandırır ve ASP.NET Core uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="13209-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="13209-136">Ters proxy sunucusu kullan</span><span class="sxs-lookup"><span data-stu-id="13209-136">Use a reverse proxy server</span></span>

<span data-ttu-id="13209-137">Kestrel'i, ASP.NET Core dinamik içerik hizmet vermek için idealdir.</span><span class="sxs-lookup"><span data-stu-id="13209-137">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="13209-138">Ancak, zengin olarak IIS, Apache veya Ngınx gibi sunucuları olarak web hizmeti özellikleri değildir.</span><span class="sxs-lookup"><span data-stu-id="13209-138">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="13209-139">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTPS sonlandırma HTTP sunucusundan sıkıştırma gibi iş boşaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13209-139">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.</span></span> <span data-ttu-id="13209-140">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="13209-140">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="13209-141">Bu kılavuzun amacı doğrultusunda, Ngınx tek bir örneği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13209-141">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="13209-142">Aynı sunucuda, HTTP sunucusu ile birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="13209-142">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="13209-143">Gereksinimlerinize bağlı olarak, bu farklı kurulum seçmiş olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13209-143">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="13209-144">Ters proxy tarafından istekleri iletilir çünkü [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="13209-144">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="13209-145">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` bu yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri doğru çalışması için başlık.</span><span class="sxs-lookup"><span data-stu-id="13209-145">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="13209-146">Kimlik doğrulaması, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi bir düzen bağlı olduğu herhangi bir bileşeni çağrılırken iletilen üstbilgileri Ara sonra yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="13209-146">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="13209-147">Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri ara yazılım çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="13209-147">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="13209-148">Bu sıralama, iletilen üst bilgi bağlı olan ara yazılım işleme için üstbilgi değerlerini tüketebileceği sağlar.</span><span class="sxs-lookup"><span data-stu-id="13209-148">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="13209-149">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulaması düzeni ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="13209-149">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="13209-150">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="13209-150">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="13209-151">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer bir kimlik doğrulama düzeni Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="13209-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="13209-152">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="13209-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="13209-153">Hayır ise [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan başlıkları `None`.</span><span class="sxs-lookup"><span data-stu-id="13209-153">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="13209-154">Yalnızca komut localhost üzerinde çalışan proxy'leri (127.0.0.1, [:: 1]) varsayılan olarak güvenilir.</span><span class="sxs-lookup"><span data-stu-id="13209-154">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="13209-155">Diğer güvenilen proxy'leri veya kuruluş tanıtıcı istekler Internet ile web sunucusu arasında ağ eklerseniz bunları listesine <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> veya <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> ile <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="13209-155">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="13209-156">Aşağıdaki örnekte güvenilen Ara sunucu IP adresi 10.0.0.100 iletilen üstbilgileri ara yazılımı için ekler `KnownProxies` içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="13209-156">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="13209-157">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="13209-157">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="13209-158">Ngınx yükleme</span><span class="sxs-lookup"><span data-stu-id="13209-158">Install Nginx</span></span>

<span data-ttu-id="13209-159">Kullanım `apt-get` Ngınx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="13209-159">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="13209-160">Yükleyici oluşturur bir *systemd* Ngınx arka plan programı sistem başlangıcında çalışan başlatma betiği.</span><span class="sxs-lookup"><span data-stu-id="13209-160">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="13209-161">Ubuntu için yükleme yönergelerini izleyin [Nginx: Debian/Ubuntu paketleri resmi](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="13209-161">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="13209-162">İsteğe bağlı Ngınx modülleri gerekiyorsa, Ngınx kaynaktan oluşturmak gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="13209-162">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="13209-163">Ngınx ilk kez yüklemenizden açıkça başlatın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="13209-163">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="13209-164">Giriş sayfası için Nginx varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13209-164">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="13209-165">Giriş sayfası ulaşılabildiğinden `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="13209-165">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="13209-166">Ngınx yapılandırma</span><span class="sxs-lookup"><span data-stu-id="13209-166">Configure Nginx</span></span>

<span data-ttu-id="13209-167">Ngınx ters Ara sunucu istekleri iletmek üzere ASP.NET Core Uygulamanızı yapılandırmak için değiştirin */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="13209-167">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="13209-168">Bir metin düzenleyicisinde açın ve içeriğini aşağıdakilerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="13209-168">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="13209-169">Hiçbir `server_name` eşleşme, Nginx varsayılan sunucu kullanır.</span><span class="sxs-lookup"><span data-stu-id="13209-169">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="13209-170">Varsayılan sunucu tanımladıysanız, ilk sunucu yapılandırma dosyasındaki varsayılan sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="13209-170">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="13209-171">En iyi uygulama, yapılandırma dosyanızdaki 444 durum kodu döndüren bir belirli varsayılan sunucu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13209-171">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="13209-172">Varsayılan sunucu yapılandırma örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="13209-172">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="13209-173">Önceki yapılandırma dosyası ve varsayılan sunucusuyla, Ngınx ana bilgisayar üst bilgisi ile 80 numaralı bağlantı noktasında ortak trafiğin kabul `example.com` veya `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="13209-173">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="13209-174">Bu konaklar eşleşmeyen istekleri için Kestrel iletilen olmaz.</span><span class="sxs-lookup"><span data-stu-id="13209-174">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="13209-175">Ngınx eşleşen istekleri sırasında Kestrel iletir `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="13209-175">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="13209-176">Bkz: [nasıl ngınx bir isteği işler](https://nginx.org/docs/http/request_processing.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="13209-176">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="13209-177">Kestrel'i'nın IP/bağlantı noktasını değiştirmek için bkz [Kestrel: Uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="13209-177">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="13209-178">Uygun belirtmek için hata [sunucu_adı yönergesi](https://nginx.org/docs/http/server_names.html) uygulamanıza güvenlik açıklarını kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="13209-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="13209-179">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanını denetimi bu güvenlik riski yoktur (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="13209-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="13209-180">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="13209-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="13209-181">Ngınx yapılandırmasında kurulduktan sonra Çalıştır `sudo nginx -t` yapılandırma dosyalarının söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13209-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="13209-182">Yapılandırma dosyası test başarılı olursa, Ngınx çalıştırarak değişikliklerini seçmek için zorlama `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="13209-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="13209-183">Doğrudan uygulama sunucusunda çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="13209-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="13209-184">Uygulamanın dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="13209-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="13209-185">Uygulamayı çalıştırın: `dotnet <app_assembly.dll>`burada `app_assembly.dll` uygulamanın derleme dosya adı.</span><span class="sxs-lookup"><span data-stu-id="13209-185">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="13209-186">Uygulama sunucu üzerinde çalışır, ancak Internet üzerinden yanıt verememesi durumunda sunucunun Güvenlik Duvarı'nı denetleyin ve bağlantı noktası 80 açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="13209-186">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="13209-187">Azure Ubuntu sanal makinesi kullanıyorsanız, gelen bağlantı noktası 80 trafiğini etkinleştirir, bir ağ güvenlik grubu (NSG) kuralı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13209-187">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="13209-188">Gelen kural etkinleştirildiğinde giden trafiği otomatik olarak verilir olarak bir giden bağlantı noktası 80 kuralını etkinleştirmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="13209-188">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="13209-189">Uygulamayı test etme işiniz bittiğinde, uygulama ile kapatma `Ctrl+C` komut isteminde.</span><span class="sxs-lookup"><span data-stu-id="13209-189">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitor-the-app"></a><span data-ttu-id="13209-190">Uygulamayı izleme</span><span class="sxs-lookup"><span data-stu-id="13209-190">Monitor the app</span></span>

<span data-ttu-id="13209-191">Sunucu yapılan istekleri iletmek üzere kurulur `http://<serveraddress>:80` sırasında Kestrel üzerinde çalışan ASP.NET Core uygulaması açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="13209-191">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="13209-192">Ancak, Ngınx Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="13209-192">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="13209-193">*systemd* başlatmak ve temel alınan web uygulamasını izleme için bir hizmet dosyası oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13209-193">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="13209-194">*systemd* başlatılmasını, durdurmasını ve işlemlerini yönetme için çok sayıda güçlü özellikler sağlar init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="13209-194">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="13209-195">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="13209-195">Create the service file</span></span>

<span data-ttu-id="13209-196">Hizmet tanım dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="13209-196">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="13209-197">Örnek bir uygulama için hizmet dosyası aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="13209-197">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="13209-198">Kullanıcı *www-veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için uygun sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="13209-198">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="13209-199">Kullanım `TimeoutStopSec` uygulama ilk kesme sinyallerini aldığı sonra kapatmak beklenecek süre yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="13209-199">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="13209-200">Uygulama bu dönemde değil kapatırsanız SIGKILL uygulamayı sonlandırmak için verilir.</span><span class="sxs-lookup"><span data-stu-id="13209-200">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="13209-201">Unitless saniye değer sağlayın (örneğin, `150`), bir zaman aralığı değeri (örneğin, `2min 30s`), veya `infinity` zaman aşımı devre dışı bırakmak için.</span><span class="sxs-lookup"><span data-stu-id="13209-201">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="13209-202">`TimeoutStopSec` değerini varsayılan olarak `DefaultTimeoutStopSec` Yöneticisi yapılandırma dosyasında (*systemd system.conf*, *system.conf.d*, *systemd user.conf*,  *User.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="13209-202">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="13209-203">Çoğu dağıtımlar için varsayılan zaman aşımı değeri 90 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="13209-203">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="13209-204">Linux, büyük küçük harfe duyarlı dosya sistemi vardır.</span><span class="sxs-lookup"><span data-stu-id="13209-204">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="13209-205">Yapılandırma dosyası arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="13209-205">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="13209-206">Ortam değişkenlerini okumak yapılandırma sağlayıcıları için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçınılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="13209-206">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="13209-207">Yapılandırma dosyasında kullanmak için düzgün bir şekilde atlanan bir değer oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="13209-207">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="13209-208">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="13209-208">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="13209-209">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="13209-209">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="13209-210">Web uygulaması yönetilen systemd Kestrel ve yapılandırılmış ters proxy, tam olarak yapılandırılır ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="13209-210">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="13209-211">Ayrıca, bir uzak makineden engelleyebilecek bir güvenlik duvarı engelleme de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="13209-211">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="13209-212">Yanıt üst bilgilerini inceleyerek `Server` üstbilgisi Kestrel tarafından sunulan ASP.NET Core uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="13209-212">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="13209-213">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="13209-213">View logs</span></span>

<span data-ttu-id="13209-214">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olayları ve işlemler için merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13209-214">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="13209-215">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="13209-215">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="13209-216">Görüntülenecek `kestrel-helloapp.service`-belirli öğeler, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="13209-216">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="13209-217">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir kombinasyonunu döndürülen girdileri miktarını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13209-217">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="13209-218">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="13209-218">Data protection</span></span>

<span data-ttu-id="13209-219">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index)kimlik doğrulaması ara yazılımı (örneğin, tanımlama bilgisi Ara) dahil olmak üzere ve siteler arası istek sahteciliği (CSRF) korumaları.</span><span class="sxs-lookup"><span data-stu-id="13209-219">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="13209-220">Bir kalıcı oluşturmak için veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="13209-220">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="13209-221">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="13209-221">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="13209-222">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="13209-222">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="13209-223">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="13209-223">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="13209-224">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="13209-224">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="13209-225">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="13209-225">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="13209-226">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="13209-226">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="13209-227">Kalıcı hale getirmek ve anahtar halkası şifrelemek için veri korumayı yapılandırmak için bkz:</span><span class="sxs-lookup"><span data-stu-id="13209-227">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a><span data-ttu-id="13209-228">Uzun bir isteği üst bilgi alanları</span><span class="sxs-lookup"><span data-stu-id="13209-228">Long request header fields</span></span>

<span data-ttu-id="13209-229">Uygulama isteği gerektiriyorsa, proxy sunucu tarafından izin verilenden daha uzun olan üstbilgi alanlarını varsayılan ayarlar (genellikle 4K ya da platforma bağlı olarak 8K), aşağıdaki yönergeleri ayarlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="13209-229">If the app requires request header fields longer than permitted by the proxy server's default settings (typically 4K or 8K depending on the platform), the following directives require adjustment.</span></span> <span data-ttu-id="13209-230">Uygulamak için değerleri senaryoya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="13209-230">The values to apply are scenario-dependent.</span></span> <span data-ttu-id="13209-231">Daha fazla bilgi için sunucunuzun belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="13209-231">For more information, see your server's documentation.</span></span>

* [<span data-ttu-id="13209-232">proxy_buffer_size</span><span class="sxs-lookup"><span data-stu-id="13209-232">proxy_buffer_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [<span data-ttu-id="13209-233">proxy_buffers</span><span class="sxs-lookup"><span data-stu-id="13209-233">proxy_buffers</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [<span data-ttu-id="13209-234">proxy_busy_buffers_size</span><span class="sxs-lookup"><span data-stu-id="13209-234">proxy_busy_buffers_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [<span data-ttu-id="13209-235">large_client_header_buffers</span><span class="sxs-lookup"><span data-stu-id="13209-235">large_client_header_buffers</span></span>](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> <span data-ttu-id="13209-236">Proxy arabellekler için varsayılan değerleri sürece artırmaz gerekli.</span><span class="sxs-lookup"><span data-stu-id="13209-236">Don't increase the default values of proxy buffers unless necessary.</span></span> <span data-ttu-id="13209-237">Bu değerleri artırma (taşma) arabellek taşması riskini artırır ve kötü amaçlı kullanıcılar tarafından hizmet reddi (DoS) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="13209-237">Increasing these values increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="secure-the-app"></a><span data-ttu-id="13209-238">Bir uygulamanın güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="13209-238">Secure the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="13209-239">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="13209-239">Enable AppArmor</span></span>

<span data-ttu-id="13209-240">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdeğinin parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="13209-240">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="13209-241">LSM güvenlik modülleri'nın farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="13209-241">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="13209-242">[AppArmor](https://wiki.ubuntu.com/AppArmor) sınırlı bir kaynak kümesini programa sınırlandırma izin veren bir zorunlu erişim denetimi sistemi uygulayan bir LSM olduğu.</span><span class="sxs-lookup"><span data-stu-id="13209-242">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="13209-243">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="13209-243">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configure-the-firewall"></a><span data-ttu-id="13209-244">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="13209-244">Configure the firewall</span></span>

<span data-ttu-id="13209-245">Kullanılmayan tüm dış bağlantı devre dışı kapatın.</span><span class="sxs-lookup"><span data-stu-id="13209-245">Close off all external ports that are not in use.</span></span> <span data-ttu-id="13209-246">Karmaşık güvenlik duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarını yapılandırmak için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="13209-246">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="13209-247">Bir güvenlik duvarı tüm sisteme erişimini engelleyecek değilse doğru yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="13209-247">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="13209-248">Bağlanmak için SSH kullanıyorsanız hata doğru SSH bağlantı noktası belirtmek için etkili bir şekilde, sistemin dışında kilitleyecek.</span><span class="sxs-lookup"><span data-stu-id="13209-248">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="13209-249">Varsayılan bağlantı noktası 22'dir.</span><span class="sxs-lookup"><span data-stu-id="13209-249">The default port is 22.</span></span> <span data-ttu-id="13209-250">Daha fazla bilgi için [ufw giriş](https://help.ubuntu.com/community/UFW) ve [el ile](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="13209-250">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="13209-251">Yükleme `ufw` ve gerekli herhangi bir bağlantı noktası üzerinde trafiğe izin verecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="13209-251">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a><span data-ttu-id="13209-252">Güvenli Ngınx</span><span class="sxs-lookup"><span data-stu-id="13209-252">Secure Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="13209-253">Ngınx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="13209-253">Change the Nginx response name</span></span>

<span data-ttu-id="13209-254">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="13209-254">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="13209-255">Seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="13209-255">Configure options</span></span>

<span data-ttu-id="13209-256">Ek gerekli modülleri ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="13209-256">Configure the server with additional required modules.</span></span> <span data-ttu-id="13209-257">Bir web uygulaması güvenlik duvarı gibi kullanmayı göz önünde bulundurun [ModSecurity](https://www.modsecurity.org/)uygulama sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="13209-257">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="https-configuration"></a><span data-ttu-id="13209-258">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="13209-258">HTTPS configuration</span></span>

* <span data-ttu-id="13209-259">Bağlantı HTTPS trafiği için dinlemek üzere yapılandırmak `443` belirterek bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika.</span><span class="sxs-lookup"><span data-stu-id="13209-259">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="13209-260">Aşağıda gösterilen yöntemler kullanarak güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="13209-260">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="13209-261">Daha güçlü şifreleme seçme ve tüm trafiği, HTTP, HTTPS üzerinden yönlendirme verilebilir.</span><span class="sxs-lookup"><span data-stu-id="13209-261">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="13209-262">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üst bilgisi olan HTTPS üzerinden istemci tarafından yapılan tüm sonraki istekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="13209-262">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="13209-263">HSTS üst bilgi eklemeyin veya uygun bir seçtiğiniz `max-age` varsa HTTPS gelecekte devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="13209-263">Don't add the HSTS header or chose an appropriate `max-age` if HTTPS will be disabled in the future.</span></span>

<span data-ttu-id="13209-264">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="13209-264">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="13209-265">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="13209-265">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="13209-266">Örnek içeren `http` ve `server` bölümlerde bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="13209-266">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="13209-267">Güvenli Ngınx clickjacking gelen</span><span class="sxs-lookup"><span data-stu-id="13209-267">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="13209-268">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)olarak da bilinen bir *UI redress saldırı*, bir kötü amaçlı bir Web sitesi ziyaretçi yere sağladı daha şu anda ziyaret ettiğiniz bir bağlantı veya başka bir sayfaya düğmesine tıklamak saldırıdır.</span><span class="sxs-lookup"><span data-stu-id="13209-268">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="13209-269">Kullanım `X-FRAME-OPTIONS` sitesini güvenli hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="13209-269">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="13209-270">Clickjacking saldırıları azaltmak için:</span><span class="sxs-lookup"><span data-stu-id="13209-270">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="13209-271">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="13209-271">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="13209-272">Satır Ekle `add_header X-Frame-Options "SAMEORIGIN";`.</span><span class="sxs-lookup"><span data-stu-id="13209-272">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="13209-273">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13209-273">Save the file.</span></span>
1. <span data-ttu-id="13209-274">Ngınx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="13209-274">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="13209-275">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="13209-275">MIME-type sniffing</span></span>

<span data-ttu-id="13209-276">Tarayıcı yanıt içerik türü geçersiz üstbilgi bildirir gibi bu başlığı çoğu tarayıcılardan MIME algılaması bildirilen içerik türü, uzakta bir yanıt engeller.</span><span class="sxs-lookup"><span data-stu-id="13209-276">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="13209-277">İle `nosniff` seçeneğinde, tarayıcı sunucu içeriktir "text/html" derse, "metin/html olarak" işleyen.</span><span class="sxs-lookup"><span data-stu-id="13209-277">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="13209-278">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="13209-278">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="13209-279">Satır Ekle `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin ve ardından Ngınx'i yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="13209-279">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13209-280">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="13209-280">Additional resources</span></span>

* [<span data-ttu-id="13209-281">Linux üzerinde .NET Core önkoşulları</span><span class="sxs-lookup"><span data-stu-id="13209-281">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* [<span data-ttu-id="13209-282">Ngınx: İkili sürümler: Debian/Ubuntu paketleri resmi</span><span class="sxs-lookup"><span data-stu-id="13209-282">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="13209-283">NGINX: İletilen üstbilgi kullanılıyor</span><span class="sxs-lookup"><span data-stu-id="13209-283">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
