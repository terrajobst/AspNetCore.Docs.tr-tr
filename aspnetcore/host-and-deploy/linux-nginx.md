---
title: ASP.NET Core Nginx ile Linux'ta barındırma
author: rick-anderson
description: Ngınx Kestrel üzerinde çalışan ASP.NET Core web uygulaması HTTP trafiği iletmek için Ubuntu 16.04 ters bir proxy olarak ayarlamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 074ff3c0db04b9ac7d32fed636a8c4cef5f06e58
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219335"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="5f341-103">ASP.NET Core Nginx ile Linux'ta barındırma</span><span class="sxs-lookup"><span data-stu-id="5f341-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="5f341-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="5f341-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="5f341-105">Bu kılavuzda bir Ubuntu 16.04 sunucusunda bir üretime hazır ASP.NET Core ortamını ayarlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5f341-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="5f341-106">Büyük olasılıkla bu yönergeleri Ubuntu daha yeni sürümleri ile çalışır, ancak yeni sürümleriyle yönergeleri test edilmemiştir.</span><span class="sxs-lookup"><span data-stu-id="5f341-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="5f341-107">Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="5f341-108">*systemd* Ubuntu 14.04 üzerinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="5f341-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="5f341-109">Ubuntu 14.04 yönergeler için bkz: [bu konunun önceki sürümü](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="5f341-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="5f341-110">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="5f341-110">This guide:</span></span>

* <span data-ttu-id="5f341-111">Mevcut bir ASP.NET Core uygulaması bir ters proxy sunucusunun arkasında yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f341-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="5f341-112">Ters proxy sunucuyu Kestrel web sunucusu istekleri iletmek için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="5f341-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="5f341-113">Web uygulaması başlangıç bir daemon olarak gerçekleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f341-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="5f341-114">Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="5f341-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f341-115">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5f341-115">Prerequisites</span></span>

1. <span data-ttu-id="5f341-116">Bir Ubuntu 16.04 sunucusuyla sudo ayrıcalıklarıyla standart kullanıcı hesabı erişimi.</span><span class="sxs-lookup"><span data-stu-id="5f341-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="5f341-117">.NET Core çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5f341-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="5f341-118">Ziyaret [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="5f341-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="5f341-119">En son Önizleme çalışma zamanı altında listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="5f341-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="5f341-120">Seçin ve Ubuntu için Ubuntu server sürümüyle eşleşen yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="5f341-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="5f341-121">Mevcut bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="5f341-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="5f341-122">Yayımlama ve uygulamanın üzerine kopyalayın</span><span class="sxs-lookup"><span data-stu-id="5f341-122">Publish and copy over the app</span></span>

<span data-ttu-id="5f341-123">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="5f341-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="5f341-124">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulamayı paketlemek için geliştirme ortamından (örneğin, *bin/yayın/&lt;target_framework_moniker&gt;/ publish*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5f341-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="5f341-125">Uygulama ayrıca olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) .NET Core çalışma zamanı sunucuda değil sağlamak isterseniz.</span><span class="sxs-lookup"><span data-stu-id="5f341-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="5f341-126">ASP.NET Core uygulaması (örneğin, SCP, SFTP) kuruluşun iş akışınıza tümleştirir bir aracını kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="5f341-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="5f341-127">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="5f341-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="5f341-128">Üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="5f341-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="5f341-129">Uygulamayı test edin:</span><span class="sxs-lookup"><span data-stu-id="5f341-129">Test the app:</span></span>

1. <span data-ttu-id="5f341-130">Uygulamayı komut satırından çalıştırın: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="5f341-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="5f341-131">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulamayı Linux üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5f341-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="5f341-132">Ters proxy sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5f341-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="5f341-133">Ters proxy hizmet dinamik web uygulamaları için ortak bir kurulum var.</span><span class="sxs-lookup"><span data-stu-id="5f341-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="5f341-134">Ters proxy, HTTP isteği sonlandırır ve ASP.NET Core uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="5f341-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="5f341-135">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="5f341-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="5f341-136">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="5f341-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="5f341-137">Ters proxy sunucusu kullan</span><span class="sxs-lookup"><span data-stu-id="5f341-137">Use a reverse proxy server</span></span>

<span data-ttu-id="5f341-138">Kestrel'i, ASP.NET Core dinamik içerik hizmet vermek için idealdir.</span><span class="sxs-lookup"><span data-stu-id="5f341-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="5f341-139">Ancak, zengin olarak IIS, Apache veya Ngınx gibi sunucuları olarak web hizmeti özellikleri değildir.</span><span class="sxs-lookup"><span data-stu-id="5f341-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="5f341-140">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekler ve SSL sonlandırma HTTP sunucusundan sıkıştırma gibi iş boşaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f341-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="5f341-141">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="5f341-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="5f341-142">Bu kılavuzun amacı doğrultusunda, Ngınx tek bir örneği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f341-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="5f341-143">Aynı sunucuda, HTTP sunucusu ile birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="5f341-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="5f341-144">Gereksinimlerinize bağlı olarak, bu farklı kurulum seçmiş olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f341-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="5f341-145">Ters proxy tarafından istekleri iletilir çünkü [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="5f341-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="5f341-146">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` bu yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri doğru çalışması için başlık.</span><span class="sxs-lookup"><span data-stu-id="5f341-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="5f341-147">Kimlik doğrulaması, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi bir düzen bağlı olduğu herhangi bir bileşeni çağrılırken iletilen üstbilgileri Ara sonra yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5f341-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="5f341-148">Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri ara yazılım çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5f341-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="5f341-149">Bu sıralama, iletilen üst bilgi bağlı olan ara yazılım işleme için üstbilgi değerlerini tüketebileceği sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f341-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5f341-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5f341-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5f341-151">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulaması düzeni ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="5f341-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="5f341-152">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="5f341-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5f341-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5f341-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5f341-154">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer bir kimlik doğrulama düzeni Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="5f341-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="5f341-155">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="5f341-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

---

<span data-ttu-id="5f341-156">Hayır ise [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan başlıkları `None`.</span><span class="sxs-lookup"><span data-stu-id="5f341-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="5f341-157">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="5f341-158">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="5f341-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="5f341-159">Ngınx yükleme</span><span class="sxs-lookup"><span data-stu-id="5f341-159">Install Nginx</span></span>

<span data-ttu-id="5f341-160">Kullanım `apt-get` Ngınx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="5f341-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="5f341-161">Yükleyici oluşturur bir *systemd* Ngınx arka plan programı sistem başlangıcında çalışan başlatma betiği.</span><span class="sxs-lookup"><span data-stu-id="5f341-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="5f341-162">Ubuntu kişisel paket arşivini (PPA) gönüllüsü tarafından korunur ve tarafından dağıtılmadı [nginx.org](https://nginx.org/). Daha fazla bilgi için [Nginx: ikili sürümleri: resmi Debian/Ubuntu paketleri](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="5f341-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="5f341-163">İsteğe bağlı Ngınx modülleri gerekiyorsa, Ngınx kaynaktan oluşturmak gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="5f341-164">Ngınx ilk kez yüklemenizden açıkça başlatın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="5f341-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="5f341-165">Giriş sayfası için Nginx varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5f341-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="5f341-166">Giriş sayfası ulaşılabildiğinden `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="5f341-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="5f341-167">Ngınx yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5f341-167">Configure Nginx</span></span>

<span data-ttu-id="5f341-168">Ngınx ters Ara sunucu istekleri iletmek üzere ASP.NET Core Uygulamanızı yapılandırmak için değiştirin */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="5f341-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="5f341-169">Bir metin düzenleyicisinde açın ve içeriğini aşağıdakilerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="5f341-169">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="5f341-170">Hiçbir `server_name` eşleşme, Nginx varsayılan sunucu kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f341-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="5f341-171">Varsayılan sunucu tanımladıysanız, ilk sunucu yapılandırma dosyasındaki varsayılan sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="5f341-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="5f341-172">En iyi uygulama, yapılandırma dosyanızdaki 444 durum kodu döndüren bir belirli varsayılan sunucu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5f341-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="5f341-173">Varsayılan sunucu yapılandırma örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5f341-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="5f341-174">Önceki yapılandırma dosyası ve varsayılan sunucusuyla, Ngınx ana bilgisayar üst bilgisi ile 80 numaralı bağlantı noktasında ortak trafiğin kabul `example.com` veya `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="5f341-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="5f341-175">Bu konaklar eşleşmeyen istekleri için Kestrel iletilen olmaz.</span><span class="sxs-lookup"><span data-stu-id="5f341-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="5f341-176">Ngınx eşleşen istekleri sırasında Kestrel iletir `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5f341-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="5f341-177">Bkz: [nasıl ngınx bir isteği işler](https://nginx.org/docs/http/request_processing.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="5f341-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="5f341-178">Kestrel'i'nın IP/bağlantı noktasını değiştirmek için bkz [Kestrel: uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="5f341-178">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="5f341-179">Uygun belirtmek için hata [sunucu_adı yönergesi](https://nginx.org/docs/http/server_names.html) uygulamanıza güvenlik açıklarını kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="5f341-179">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="5f341-180">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanını denetimi bu güvenlik riski yoktur (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="5f341-180">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="5f341-181">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="5f341-181">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="5f341-182">Ngınx yapılandırmasında kurulduktan sonra Çalıştır `sudo nginx -t` yapılandırma dosyalarının söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5f341-182">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="5f341-183">Yapılandırma dosyası test başarılı olursa, Ngınx çalıştırarak değişikliklerini seçmek için zorlama `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="5f341-183">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="5f341-184">Doğrudan uygulama sunucusunda çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="5f341-184">To directly run the app on the server:</span></span>

1. <span data-ttu-id="5f341-185">Uygulamanın dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="5f341-185">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="5f341-186">Uygulamanın yürütülebilir dosyayı çalıştırmak: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="5f341-186">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="5f341-187">İzin hatası meydana gelirse, izinleri değiştirin:</span><span class="sxs-lookup"><span data-stu-id="5f341-187">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="5f341-188">Uygulama sunucu üzerinde çalışır, ancak Internet üzerinden yanıt verememesi durumunda sunucunun Güvenlik Duvarı'nı denetleyin ve bağlantı noktası 80 açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="5f341-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="5f341-189">Azure Ubuntu sanal makinesi kullanıyorsanız, gelen bağlantı noktası 80 trafiğini etkinleştirir, bir ağ güvenlik grubu (NSG) kuralı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5f341-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="5f341-190">Gelen kural etkinleştirildiğinde giden trafiği otomatik olarak verilir olarak bir giden bağlantı noktası 80 kuralını etkinleştirmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="5f341-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="5f341-191">Uygulamayı test etme işiniz bittiğinde, uygulama ile kapatma `Ctrl+C` komut isteminde.</span><span class="sxs-lookup"><span data-stu-id="5f341-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="5f341-192">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="5f341-192">Monitoring the app</span></span>

<span data-ttu-id="5f341-193">Sunucu yapılan istekleri iletmek üzere kurulur `http://<serveraddress>:80` sırasında Kestrel üzerinde çalışan ASP.NET Core uygulaması açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="5f341-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="5f341-194">Ancak, Ngınx Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="5f341-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="5f341-195">*systemd* başlatmak ve temel alınan web uygulamasını izleme için bir hizmet dosyası oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="5f341-196">*systemd* başlatılmasını, durdurmasını ve işlemlerini yönetme için çok sayıda güçlü özellikler sağlar init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="5f341-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="5f341-197">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="5f341-197">Create the service file</span></span>

<span data-ttu-id="5f341-198">Hizmet tanım dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="5f341-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="5f341-199">Örnek bir uygulama için hizmet dosyası aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5f341-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="5f341-200">Kullanıcı *www-veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için uygun sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="5f341-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="5f341-201">Linux, büyük küçük harfe duyarlı dosya sistemi vardır.</span><span class="sxs-lookup"><span data-stu-id="5f341-201">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="5f341-202">Yapılandırma dosyası arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="5f341-202">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="5f341-203">Ortam değişkenlerini okumak yapılandırma sağlayıcıları için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçınılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5f341-203">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="5f341-204">Yapılandırma dosyasında kullanmak için düzgün bir şekilde atlanan bir değer oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="5f341-204">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="5f341-205">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="5f341-205">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="5f341-206">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="5f341-206">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="5f341-207">Web uygulaması yönetilen systemd Kestrel ve yapılandırılmış ters proxy, tam olarak yapılandırılır ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="5f341-207">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="5f341-208">Ayrıca, bir uzak makineden engelleyebilecek bir güvenlik duvarı engelleme de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-208">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="5f341-209">Yanıt üst bilgilerini inceleyerek `Server` üstbilgisi Kestrel tarafından sunulan ASP.NET Core uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="5f341-209">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="5f341-210">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="5f341-210">Viewing logs</span></span>

<span data-ttu-id="5f341-211">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olayları ve işlemler için merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-211">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="5f341-212">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="5f341-212">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="5f341-213">Görüntülenecek `kestrel-hellomvc.service`-belirli öğeler, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="5f341-213">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="5f341-214">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir kombinasyonunu döndürülen girdileri miktarını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f341-214">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="5f341-215">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="5f341-215">Data protection</span></span>

<span data-ttu-id="5f341-216">[ASP.NET Core veri koruma yığın](xref:security/data-protection/index) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index)kimlik doğrulaması ara yazılımı (örneğin, tanımlama bilgisi Ara) dahil olmak üzere ve siteler arası istek sahteciliği (CSRF) korumaları.</span><span class="sxs-lookup"><span data-stu-id="5f341-216">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="5f341-217">Bir kalıcı oluşturmak için veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="5f341-217">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="5f341-218">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="5f341-218">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="5f341-219">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="5f341-219">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="5f341-220">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="5f341-220">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="5f341-221">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5f341-221">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="5f341-222">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-222">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="5f341-223">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="5f341-223">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="5f341-224">Kalıcı hale getirmek ve anahtar halkası şifrelemek için veri korumayı yapılandırmak için bkz:</span><span class="sxs-lookup"><span data-stu-id="5f341-224">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="5f341-225">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="5f341-225">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="5f341-226">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="5f341-226">Enable AppArmor</span></span>

<span data-ttu-id="5f341-227">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdeğinin parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="5f341-227">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="5f341-228">LSM güvenlik modülleri'nın farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="5f341-228">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="5f341-229">[AppArmor](https://wiki.ubuntu.com/AppArmor) sınırlı bir kaynak kümesini programa sınırlandırma izin veren bir zorunlu erişim denetimi sistemi uygulayan bir LSM olduğu.</span><span class="sxs-lookup"><span data-stu-id="5f341-229">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="5f341-230">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="5f341-230">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="5f341-231">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5f341-231">Configuring the firewall</span></span>

<span data-ttu-id="5f341-232">Kullanılmayan tüm dış bağlantı devre dışı kapatın.</span><span class="sxs-lookup"><span data-stu-id="5f341-232">Close off all external ports that are not in use.</span></span> <span data-ttu-id="5f341-233">Karmaşık güvenlik duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarını yapılandırmak için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="5f341-233">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="5f341-234">Doğrulayın `ufw` gereken herhangi bir bağlantı noktası üzerinde trafiğe izin verecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="5f341-234">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="5f341-235">Ngınx'in güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="5f341-235">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="5f341-236">Ngınx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="5f341-236">Change the Nginx response name</span></span>

<span data-ttu-id="5f341-237">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="5f341-237">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="5f341-238">Seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="5f341-238">Configure options</span></span>

<span data-ttu-id="5f341-239">Ek gerekli modülleri ile yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5f341-239">Configure the server with additional required modules.</span></span> <span data-ttu-id="5f341-240">Bir web uygulaması güvenlik duvarı gibi kullanmayı göz önünde bulundurun [ModSecurity](https://www.modsecurity.org/)uygulama sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="5f341-240">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="5f341-241">SSL'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5f341-241">Configure SSL</span></span>

* <span data-ttu-id="5f341-242">Bağlantı HTTPS trafiği için dinlemek üzere yapılandırmak `443` belirterek bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika.</span><span class="sxs-lookup"><span data-stu-id="5f341-242">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="5f341-243">Aşağıda gösterilen yöntemler kullanarak güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="5f341-243">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="5f341-244">Daha güçlü şifreleme seçme ve tüm trafiği, HTTP, HTTPS üzerinden yönlendirme verilebilir.</span><span class="sxs-lookup"><span data-stu-id="5f341-244">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="5f341-245">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üst bilgisi, istemci tarafından yapılan tüm istekler, HTTPS üzerinden yalnızca sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f341-245">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="5f341-246">Katı aktarım güvenliği üst bilgi eklemeyin veya uygun bir seçtiğiniz `max-age` , SSL gelecekte devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="5f341-246">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="5f341-247">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="5f341-247">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="5f341-248">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="5f341-248">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="5f341-249">Örnek içeren `http` ve `server` bölümlerde bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="5f341-249">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="5f341-250">Güvenli Ngınx clickjacking gelen</span><span class="sxs-lookup"><span data-stu-id="5f341-250">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="5f341-251">Clickjacking etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="5f341-251">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="5f341-252">Clickjacking, etkilenen bir sitede bir öğeye tıklamak (ziyaretçi) victim ipuçları.</span><span class="sxs-lookup"><span data-stu-id="5f341-252">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="5f341-253">Kullanım X-FRAME-sitesini güvenli hale getirmek için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="5f341-253">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="5f341-254">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5f341-254">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="5f341-255">Satır Ekle `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin ve ardından Ngınx'i yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="5f341-255">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="5f341-256">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="5f341-256">MIME-type sniffing</span></span>

<span data-ttu-id="5f341-257">Tarayıcı yanıt içerik türü geçersiz üstbilgi bildirir gibi bu başlığı çoğu tarayıcılardan MIME algılaması bildirilen içerik türü, uzakta bir yanıt engeller.</span><span class="sxs-lookup"><span data-stu-id="5f341-257">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="5f341-258">İle `nosniff` seçeneğinde, tarayıcı sunucu içeriktir "text/html" derse, "metin/html olarak" işleyen.</span><span class="sxs-lookup"><span data-stu-id="5f341-258">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="5f341-259">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5f341-259">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="5f341-260">Satır Ekle `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin ve ardından Ngınx'i yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="5f341-260">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f341-261">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5f341-261">Additional resources</span></span>

* [<span data-ttu-id="5f341-262">Ngınx: İkili sürümleri: resmi Debian/Ubuntu paketleri</span><span class="sxs-lookup"><span data-stu-id="5f341-262">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="5f341-263">ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5f341-263">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="5f341-264">NGINX: iletilen üstbilgi kullanma</span><span class="sxs-lookup"><span data-stu-id="5f341-264">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
