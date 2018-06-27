---
title: ASP.NET Core Nginx ile Linux ana bilgisayar
author: rick-anderson
description: Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu Nginx Kurulum öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 0ccc9e396ffc9f7af93d5601fee0182d9e3471f4
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961496"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="8a47c-103">ASP.NET Core Nginx ile Linux ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="8a47c-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="8a47c-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="8a47c-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="8a47c-105">Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar.</span><span class="sxs-lookup"><span data-stu-id="8a47c-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="8a47c-106">Büyük olasılıkla bu yönergeleri Ubuntu daha yeni sürümleri ile çalışır ancak yönergeleri içeren daha yeni sürümlerle test edilmemiştir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="8a47c-107">Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="8a47c-108">*systemd* Ubuntu 14.04 üzerinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="8a47c-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="8a47c-109">Ubuntu 14.04 yönergeler için bkz: [bu konunun önceki sürümü](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="8a47c-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="8a47c-110">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="8a47c-110">This guide:</span></span>

* <span data-ttu-id="8a47c-111">Var olan bir ASP.NET Core uygulamaya bir ters Ara sunucu arkasındaki yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="8a47c-112">Kestrel web sunucusu isteklerini iletmek için ters proxy sunucuyu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8a47c-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="8a47c-113">Web uygulaması başlangıçta bir arka plan programı olarak gerçekleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="8a47c-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="8a47c-114">Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8a47c-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a47c-115">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8a47c-115">Prerequisites</span></span>

1. <span data-ttu-id="8a47c-116">Ubuntu 16.04 server sudo ayrıcalığa sahip standart kullanıcı hesabı ile erişim.</span><span class="sxs-lookup"><span data-stu-id="8a47c-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="8a47c-117">.NET çekirdeği çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="8a47c-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="8a47c-118">Ziyaret [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="8a47c-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="8a47c-119">En son Önizleme olmayan çalışma zamanı altındaki listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="8a47c-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="8a47c-120">Seçin ve Ubuntu için sunucu Ubuntu sürümüyle eşleşen yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="8a47c-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="8a47c-121">Mevcut bir ASP.NET Core uygulama.</span><span class="sxs-lookup"><span data-stu-id="8a47c-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="8a47c-122">Yayımlama ve uygulama kopyalayın</span><span class="sxs-lookup"><span data-stu-id="8a47c-122">Publish and copy over the app</span></span>

<span data-ttu-id="8a47c-123">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="8a47c-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="8a47c-124">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulama paketi için geliştirme ortamı'ndan (örneğin, *bin/sürüm/&lt;target_framework_moniker&gt;/ yayımlama*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8a47c-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="8a47c-125">Uygulama aynı zamanda olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) sunucuda .NET çekirdeği çalışma zamanı sürekli olmayan tercih ederseniz.</span><span class="sxs-lookup"><span data-stu-id="8a47c-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="8a47c-126">ASP.NET Core uygulama (örneğin, SCP, SFTP) kuruluşunuzun akışına tümleşen bir aracı kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="8a47c-127">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="8a47c-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="8a47c-128">Bir üretim dağıtım senaryosunda sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="8a47c-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="8a47c-129">Uygulamayı test etme:</span><span class="sxs-lookup"><span data-stu-id="8a47c-129">Test the app:</span></span>

1. <span data-ttu-id="8a47c-130">Komut satırından uygulamayı çalıştırın: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="8a47c-131">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulaması çalışır Linux üzerinde yerel olarak doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="8a47c-132">Ters proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8a47c-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="8a47c-133">Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="8a47c-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="8a47c-134">Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="8a47c-135">Her iki yapılandırma&mdash;ile veya bir ters Ara sunucu olmadan&mdash;geçerli ve desteklenen bir barındırma yapılandırması ASP.NET Core 2.0 veya sonraki uygulamalar içindir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="8a47c-136">Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="8a47c-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="8a47c-137">Ters proxy sunucusu kullan</span><span class="sxs-lookup"><span data-stu-id="8a47c-137">Use a reverse proxy server</span></span>

<span data-ttu-id="8a47c-138">Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için mükemmeldir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="8a47c-139">Bununla birlikte, IIS, Apache veya Nginx gibi sunucuları olarak özellik zengin olarak web hizmet yetenekleri değil.</span><span class="sxs-lookup"><span data-stu-id="8a47c-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="8a47c-140">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="8a47c-141">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="8a47c-142">Bu kılavuzun amaçları Nginx tek bir örneğini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a47c-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="8a47c-143">HTTP sunucusu yanında aynı sunucu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="8a47c-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="8a47c-144">Gereksinimlerine bağlı olarak, farklı kurulum seçmiş olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a47c-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="8a47c-145">İstekleri tarafından ters proxy iletilir olduğundan [iletilen üstbilgileri Ara](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="8a47c-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="8a47c-146">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="8a47c-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="8a47c-147">Kimlik doğrulama, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi şema bağımlı herhangi bir bileşeni iletilen üstbilgileri Ara başlatma sonrasında yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="8a47c-148">Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri Ara çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="8a47c-149">Bu sıralama iletilen üstbilgileri bilgi bağlı olan ara yazılım işleme üstbilgi değerleri tüketebileceği sağlar.</span><span class="sxs-lookup"><span data-stu-id="8a47c-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a47c-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a47c-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8a47c-151">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulama düzeni ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="8a47c-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="8a47c-152">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="8a47c-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a47c-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a47c-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8a47c-154">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer kimlik doğrulama şeması Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="8a47c-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="8a47c-155">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="8a47c-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="8a47c-156">Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan üstbilgiler `None`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="8a47c-157">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="8a47c-158">Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="8a47c-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="8a47c-159">Nginx yükleyin</span><span class="sxs-lookup"><span data-stu-id="8a47c-159">Install Nginx</span></span>

<span data-ttu-id="8a47c-160">Kullanım `apt-get` Nginx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="8a47c-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="8a47c-161">Yükleyici oluşturur bir *systemd* Nginx arka plan programı gibi sistem başlangıcında çalışan init komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="8a47c-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="8a47c-162">Ubuntu kişisel paket arşiv (PPA) gönüllüsü tarafından korunur ve tarafından dağıtılmadı [nginx.org](https://nginx.org/). Daha fazla bilgi için bkz: [Nginx: ikili sürümler: resmi Debian/Ubuntu paketleri](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="8a47c-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="8a47c-163">İsteğe bağlı Nginx modülleri gerekirse, Nginx kaynağından derleme gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="8a47c-164">Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="8a47c-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="8a47c-165">Giriş sayfasında Nginx için varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="8a47c-166">Giriş sayfası erişilebildiğinden `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="8a47c-167">Nginx yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8a47c-167">Configure Nginx</span></span>

<span data-ttu-id="8a47c-168">ASP.NET Core uygulamanıza istekleri iletmek için ters Ara sunucu Nginx yapılandırmak için değiştirin */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="8a47c-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="8a47c-169">Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8a47c-169">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="8a47c-170">Durumlarda `server_name` eşleşirse, Nginx varsayılan sunucu kullanır.</span><span class="sxs-lookup"><span data-stu-id="8a47c-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="8a47c-171">Varsayılan sunucu tanımlanmışsa ilk yapılandırma dosyasındaki varsayılan sunucu sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="8a47c-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="8a47c-172">En iyi uygulama, yapılandırma dosyanızda 444 bir durum kodunu döndüren bir belirli varsayılan sunucu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8a47c-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="8a47c-173">Varsayılan sunucu yapılandırma örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8a47c-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="8a47c-174">Önceki yapılandırma dosyası ve varsayılan sunucusuyla, Nginx ana bilgisayar üstbilgisi ile 80 numaralı bağlantı noktasında genel trafiği kabul `example.com` veya `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="8a47c-175">Bu konaklar eşleşmeyen istekleri için Kestrel iletilen olmaz.</span><span class="sxs-lookup"><span data-stu-id="8a47c-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="8a47c-176">Nginx Kestrel eşleşen isteklerini iletir `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="8a47c-177">Bkz: [nasıl nginx bir isteği işler](https://nginx.org/docs/http/request_processing.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8a47c-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="8a47c-178">Uygun belirtmek için hata [sunucu_adı yönergesi](https://nginx.org/docs/http/server_names.html) güvenlik açıkları, uygulamanızın kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="8a47c-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="8a47c-179">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanı denetlemek, bu güvenlik riski değil (tersine `*.com`, açık olduğu).</span><span class="sxs-lookup"><span data-stu-id="8a47c-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="8a47c-180">Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8a47c-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="8a47c-181">Nginx yapılandırma kurulduktan sonra çalıştırmak `sudo nginx -t` yapılandırma dosyalarını söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="8a47c-182">Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için Nginx zorla `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="8a47c-183">Doğrudan uygulama sunucusunda çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="8a47c-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="8a47c-184">Uygulamanın dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="8a47c-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="8a47c-185">Uygulamanın yürütülebilir dosyayı çalıştırmak: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-185">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="8a47c-186">İzin hatası oluşursa, izinleri değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8a47c-186">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="8a47c-187">Uygulama sunucusunda çalışır ancak Internet üzerinden yanıt başarısız olursa, sunucunun Güvenlik Duvarı'nı denetleyin ve 80 numaralı bağlantı noktası açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8a47c-187">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="8a47c-188">Bir Azure Ubuntu VM kullanıyorsanız, gelen bağlantı noktası 80 trafiğini etkinleştirir, bir ağ güvenlik grubu (NSG) kuralı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8a47c-188">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="8a47c-189">Giden trafik gelen kuralı etkinleştirildiğinde otomatik olarak verilir gibi bir giden bağlantı noktası 80 Kuralı etkinleştirmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="8a47c-189">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="8a47c-190">Uygulamayı test etme işiniz bittiğinde, uygulama ile kapatıldı `Ctrl+C` komut isteminde.</span><span class="sxs-lookup"><span data-stu-id="8a47c-190">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="8a47c-191">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="8a47c-191">Monitoring the app</span></span>

<span data-ttu-id="8a47c-192">Yapılan isteklerini iletmek için Kurulum sunucusudur `http://<serveraddress>:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-192">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="8a47c-193">Ancak, Nginx Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="8a47c-193">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="8a47c-194">*systemd* başlatmak ve temel web uygulaması izleme için bir hizmet dosyasını oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-194">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="8a47c-195">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-195">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="8a47c-196">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="8a47c-196">Create the service file</span></span>

<span data-ttu-id="8a47c-197">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="8a47c-197">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="8a47c-198">Uygulama için bir örnek hizmet dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8a47c-198">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="8a47c-199">Kullanıcı *www veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="8a47c-199">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="8a47c-200">Linux büyük küçük harfe duyarlı dosya sistemi vardır.</span><span class="sxs-lookup"><span data-stu-id="8a47c-200">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="8a47c-201">Yapılandırma dosyası için arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="8a47c-201">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="8a47c-202">Ortam değişkenleri okumak yapılandırma sağlayıcısı için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçış uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8a47c-202">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="8a47c-203">Yapılandırma dosyasında kullanmak için düzgün bir şekilde Atlanan değeri oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="8a47c-203">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="8a47c-204">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="8a47c-204">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="8a47c-205">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="8a47c-205">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="8a47c-206">Ters proxy yapılandırılmış ve systemd yönetilen Kestrel ile web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-206">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="8a47c-207">Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-207">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="8a47c-208">Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-208">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="8a47c-209">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="8a47c-209">Viewing logs</span></span>

<span data-ttu-id="8a47c-210">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-210">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="8a47c-211">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="8a47c-211">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="8a47c-212">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="8a47c-212">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="8a47c-213">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.</span><span class="sxs-lookup"><span data-stu-id="8a47c-213">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="8a47c-214">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="8a47c-214">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="8a47c-215">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="8a47c-215">Enable AppArmor</span></span>

<span data-ttu-id="8a47c-216">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-216">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="8a47c-217">LSM güvenlik modüllerin farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="8a47c-217">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="8a47c-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil.</span><span class="sxs-lookup"><span data-stu-id="8a47c-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="8a47c-219">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="8a47c-219">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="8a47c-220">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8a47c-220">Configuring the firewall</span></span>

<span data-ttu-id="8a47c-221">Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-221">Close off all external ports that are not in use.</span></span> <span data-ttu-id="8a47c-222">Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="8a47c-222">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="8a47c-223">Doğrulayın `ufw` gereken herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="8a47c-223">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="8a47c-224">Nginx güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="8a47c-224">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="8a47c-225">Nginx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="8a47c-225">Change the Nginx response name</span></span>

<span data-ttu-id="8a47c-226">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="8a47c-226">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="8a47c-227">Seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8a47c-227">Configure options</span></span>

<span data-ttu-id="8a47c-228">Sunucu ile ek gerekli modüllerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-228">Configure the server with additional required modules.</span></span> <span data-ttu-id="8a47c-229">Bir web uygulaması güvenlik duvarı gibi kullanmayı [ModSecurity](https://www.modsecurity.org/), uygulama sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="8a47c-229">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="8a47c-230">SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8a47c-230">Configure SSL</span></span>

* <span data-ttu-id="8a47c-231">Bağlantı noktasında HTTPS trafiğini dinlemek üzere yapılandırmak `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.</span><span class="sxs-lookup"><span data-stu-id="8a47c-231">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="8a47c-232">Aşağıda gösterilen uygulamaları kullanan güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="8a47c-232">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="8a47c-233">Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-233">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="8a47c-234">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.</span><span class="sxs-lookup"><span data-stu-id="8a47c-234">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="8a47c-235">Strict Aktarım güvenlik üstbilgisi ekleme veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışıysa.</span><span class="sxs-lookup"><span data-stu-id="8a47c-235">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="8a47c-236">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="8a47c-236">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="8a47c-237">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="8a47c-237">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="8a47c-238">Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="8a47c-238">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="8a47c-239">Clickjacking öğesini gelen güvenli Nginx</span><span class="sxs-lookup"><span data-stu-id="8a47c-239">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="8a47c-240">Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="8a47c-240">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="8a47c-241">Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları.</span><span class="sxs-lookup"><span data-stu-id="8a47c-241">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="8a47c-242">Kullanım X-FRAME-site güvenli hale getirmek için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="8a47c-242">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="8a47c-243">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8a47c-243">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="8a47c-244">Satırı ekleyin `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-244">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="8a47c-245">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="8a47c-245">MIME-type sniffing</span></span>

<span data-ttu-id="8a47c-246">Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması çoğu tarayıcılarından yanıt içerik türü, yöntemi bir kenara bırakarak engeller.</span><span class="sxs-lookup"><span data-stu-id="8a47c-246">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="8a47c-247">İle `nosniff` seçeneği, sunucunun içeriktir "text/html" diyorsa, tarayıcı, "text/html" işler.</span><span class="sxs-lookup"><span data-stu-id="8a47c-247">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="8a47c-248">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8a47c-248">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="8a47c-249">Satırı ekleyin `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="8a47c-249">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a47c-250">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8a47c-250">Additional resources</span></span>

* [<span data-ttu-id="8a47c-251">Nginx: İkili sürümler: resmi Debian/Ubuntu paketleri</span><span class="sxs-lookup"><span data-stu-id="8a47c-251">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="8a47c-252">Proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8a47c-252">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="8a47c-253">NGINX: iletilen üstbilgi kullanma</span><span class="sxs-lookup"><span data-stu-id="8a47c-253">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
