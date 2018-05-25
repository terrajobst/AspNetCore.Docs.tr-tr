---
title: ASP.NET Core Nginx ile Linux ana bilgisayar
author: rick-anderson
description: Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu Nginx Kurulum öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: cf8965131669b681e9477113953ed40cd81df884
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="51a9d-103">ASP.NET Core Nginx ile Linux ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="51a9d-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="51a9d-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="51a9d-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="51a9d-105">Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar.</span><span class="sxs-lookup"><span data-stu-id="51a9d-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="51a9d-106">Büyük olasılıkla bu yönergeleri Ubuntu daha yeni sürümleri ile çalışır ancak yönergeleri içeren daha yeni sürümlerle test edilmemiştir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="51a9d-107">Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="51a9d-108">*systemd* Ubuntu 14.04 üzerinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="51a9d-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="51a9d-109">Ubuntu 14.04 yönergeler için bkz: [bu konunun önceki sürümü](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="51a9d-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="51a9d-110">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="51a9d-110">This guide:</span></span>

* <span data-ttu-id="51a9d-111">Var olan bir ASP.NET Core uygulamaya bir ters Ara sunucu arkasındaki yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="51a9d-112">Kestrel web sunucusu isteklerini iletmek için ters proxy sunucuyu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="51a9d-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="51a9d-113">Web uygulaması başlangıçta bir arka plan programı olarak gerçekleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="51a9d-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="51a9d-114">Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="51a9d-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51a9d-115">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="51a9d-115">Prerequisites</span></span>

1. <span data-ttu-id="51a9d-116">Ubuntu 16.04 server sudo ayrıcalığa sahip standart kullanıcı hesabı ile erişim.</span><span class="sxs-lookup"><span data-stu-id="51a9d-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="51a9d-117">.NET çekirdeği çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="51a9d-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="51a9d-118">Ziyaret [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="51a9d-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="51a9d-119">En son Önizleme olmayan çalışma zamanı altındaki listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="51a9d-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="51a9d-120">Seçin ve Ubuntu için sunucu Ubuntu sürümüyle eşleşen yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="51a9d-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="51a9d-121">Mevcut bir ASP.NET Core uygulama.</span><span class="sxs-lookup"><span data-stu-id="51a9d-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="51a9d-122">Yayımlama ve uygulama kopyalayın</span><span class="sxs-lookup"><span data-stu-id="51a9d-122">Publish and copy over the app</span></span>

<span data-ttu-id="51a9d-123">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="51a9d-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="51a9d-124">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulama paketi için geliştirme ortamı'ndan (örneğin, *bin/sürüm/&lt;target_framework_moniker&gt;/ yayımlama*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="51a9d-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="51a9d-125">Uygulama aynı zamanda olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) sunucuda .NET çekirdeği çalışma zamanı sürekli olmayan tercih ederseniz.</span><span class="sxs-lookup"><span data-stu-id="51a9d-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="51a9d-126">ASP.NET Core uygulama (örneğin, SCP, SFTP) kuruluşunuzun akışına tümleşen bir aracı kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="51a9d-127">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="51a9d-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="51a9d-128">Bir üretim dağıtım senaryosunda sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="51a9d-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="51a9d-129">Uygulamayı test etme:</span><span class="sxs-lookup"><span data-stu-id="51a9d-129">Test the app:</span></span>

1. <span data-ttu-id="51a9d-130">Komut satırından uygulamayı çalıştırın: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="51a9d-131">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulaması çalışır Linux üzerinde yerel olarak doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="51a9d-132">Ters proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="51a9d-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="51a9d-133">Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="51a9d-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="51a9d-134">Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="51a9d-135">Her iki yapılandırma&mdash;ile veya bir ters Ara sunucu olmadan&mdash;geçerli ve desteklenen bir barındırma yapılandırması ASP.NET Core 2.0 veya sonraki uygulamalar içindir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="51a9d-136">Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="51a9d-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="51a9d-137">Ters proxy sunucusu kullan</span><span class="sxs-lookup"><span data-stu-id="51a9d-137">Use a reverse proxy server</span></span>

<span data-ttu-id="51a9d-138">Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için mükemmeldir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="51a9d-139">Bununla birlikte, IIS, Apache veya Nginx gibi sunucuları olarak özellik zengin olarak web hizmet yetenekleri değil.</span><span class="sxs-lookup"><span data-stu-id="51a9d-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="51a9d-140">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="51a9d-141">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="51a9d-142">Bu kılavuzun amaçları Nginx tek bir örneğini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51a9d-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="51a9d-143">HTTP sunucusu yanında aynı sunucu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="51a9d-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="51a9d-144">Gereksinimlerine bağlı olarak, farklı kurulum seçmiş olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51a9d-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="51a9d-145">İstekleri tarafından ters proxy iletilir çünkü iletilen üstbilgileri Ara kullanmak [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="51a9d-145">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="51a9d-146">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="51a9d-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="51a9d-147">Kimlik doğrulaması ara yazılımı herhangi bir türde kullanırken, iletilen üstbilgileri Ara ilk çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-147">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="51a9d-148">Bu sıralama, kimlik doğrulaması ara yazılımı üstbilgi değerlerini kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="51a9d-148">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="51a9d-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="51a9d-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="51a9d-150">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulama düzeni ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="51a9d-150">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="51a9d-151">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="51a9d-151">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="51a9d-152">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="51a9d-152">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="51a9d-153">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer kimlik doğrulama şeması Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="51a9d-153">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="51a9d-154">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="51a9d-154">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="51a9d-155">Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan üstbilgiler `None`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-155">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="51a9d-156">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-156">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="51a9d-157">Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="51a9d-157">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="51a9d-158">Nginx yükleyin</span><span class="sxs-lookup"><span data-stu-id="51a9d-158">Install Nginx</span></span>

<span data-ttu-id="51a9d-159">Kullanım `apt-get` Nginx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="51a9d-159">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="51a9d-160">Yükleyici oluşturur bir *systemd* Nginx arka plan programı gibi sistem başlangıcında çalışan init komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="51a9d-160">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="51a9d-161">Ubuntu kişisel paket arşiv (PPA) gönüllüsü tarafından korunur ve tarafından dağıtılmadı [nginx.org](https://nginx.org/). Daha fazla bilgi için bkz: [Nginx: ikili sürümler: resmi Debian/Ubuntu paketleri](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="51a9d-161">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="51a9d-162">İsteğe bağlı Nginx modülleri gerekirse, Nginx kaynağından derleme gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-162">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="51a9d-163">Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="51a9d-163">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="51a9d-164">Giriş sayfasında Nginx için varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-164">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="51a9d-165">Giriş sayfası erişilebildiğinden `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-165">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="51a9d-166">Nginx yapılandırın</span><span class="sxs-lookup"><span data-stu-id="51a9d-166">Configure Nginx</span></span>

<span data-ttu-id="51a9d-167">ASP.NET Core uygulamanıza istekleri iletmek için ters Ara sunucu Nginx yapılandırmak için değiştirin */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="51a9d-167">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="51a9d-168">Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="51a9d-168">Open it in a text editor, and replace the contents with the following:</span></span>

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
    }
}
```

<span data-ttu-id="51a9d-169">Durumlarda `server_name` eşleşirse, Nginx varsayılan sunucu kullanır.</span><span class="sxs-lookup"><span data-stu-id="51a9d-169">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="51a9d-170">Varsayılan sunucu tanımlanmışsa ilk yapılandırma dosyasındaki varsayılan sunucu sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="51a9d-170">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="51a9d-171">En iyi uygulama, yapılandırma dosyanızda 444 bir durum kodunu döndüren bir belirli varsayılan sunucu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="51a9d-171">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="51a9d-172">Varsayılan sunucu yapılandırma örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="51a9d-172">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="51a9d-173">Önceki yapılandırma dosyası ve varsayılan sunucusuyla, Nginx ana bilgisayar üstbilgisi ile 80 numaralı bağlantı noktasında genel trafiği kabul `example.com` veya `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-173">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="51a9d-174">Bu konaklar eşleşmeyen istekleri için Kestrel iletilen olmaz.</span><span class="sxs-lookup"><span data-stu-id="51a9d-174">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="51a9d-175">Nginx Kestrel eşleşen isteklerini iletir `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-175">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="51a9d-176">Bkz: [nasıl nginx bir isteği işler](https://nginx.org/docs/http/request_processing.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="51a9d-176">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="51a9d-177">Uygun belirtmek için hata [sunucu_adı yönergesi](https://nginx.org/docs/http/server_names.html) güvenlik açıkları, uygulamanızın kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="51a9d-177">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="51a9d-178">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanı denetlemek, bu güvenlik riski değil (tersine `*.com`, açık olduğu).</span><span class="sxs-lookup"><span data-stu-id="51a9d-178">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="51a9d-179">Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="51a9d-179">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="51a9d-180">Nginx yapılandırma kurulduktan sonra çalıştırmak `sudo nginx -t` yapılandırma dosyalarını söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-180">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="51a9d-181">Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için Nginx zorla `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-181">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="51a9d-182">Doğrudan uygulama sunucusunda çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="51a9d-182">To directly run the app on the server:</span></span>

1. <span data-ttu-id="51a9d-183">Uygulamanın dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="51a9d-183">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="51a9d-184">Uygulamanın yürütülebilir dosyayı çalıştırmak: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-184">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="51a9d-185">İzin hatası oluşursa, izinleri değiştirin:</span><span class="sxs-lookup"><span data-stu-id="51a9d-185">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="51a9d-186">Uygulama sunucusunda çalışır ancak Internet üzerinden yanıt başarısız olursa, sunucunun Güvenlik Duvarı'nı denetleyin ve 80 numaralı bağlantı noktası açık olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="51a9d-186">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="51a9d-187">Bir Azure Ubuntu VM kullanıyorsanız, gelen bağlantı noktası 80 trafiğini etkinleştirir, bir ağ güvenlik grubu (NSG) kuralı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="51a9d-187">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="51a9d-188">Giden trafik gelen kuralı etkinleştirildiğinde otomatik olarak verilir gibi bir giden bağlantı noktası 80 Kuralı etkinleştirmek için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="51a9d-188">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="51a9d-189">Uygulamayı test etme işiniz bittiğinde, uygulama ile kapatıldı `Ctrl+C` komut isteminde.</span><span class="sxs-lookup"><span data-stu-id="51a9d-189">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="51a9d-190">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="51a9d-190">Monitoring the app</span></span>

<span data-ttu-id="51a9d-191">Yapılan isteklerini iletmek için Kurulum sunucusudur `http://<serveraddress>:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-191">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="51a9d-192">Ancak, Nginx Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="51a9d-192">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="51a9d-193">*systemd* başlatmak ve temel web uygulaması izleme için bir hizmet dosyasını oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-193">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="51a9d-194">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-194">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="51a9d-195">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="51a9d-195">Create the service file</span></span>

<span data-ttu-id="51a9d-196">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="51a9d-196">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="51a9d-197">Uygulama için bir örnek hizmet dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="51a9d-197">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="51a9d-198">Kullanıcı *www veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="51a9d-198">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="51a9d-199">Linux büyük küçük harfe duyarlı dosya sistemi vardır.</span><span class="sxs-lookup"><span data-stu-id="51a9d-199">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="51a9d-200">Yapılandırma dosyası için arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="51a9d-200">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="51a9d-201">Ortam değişkenleri okumak yapılandırma sağlayıcısı için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçış uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="51a9d-201">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="51a9d-202">Yapılandırma dosyasında kullanmak için düzgün bir şekilde Atlanan değeri oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="51a9d-202">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="51a9d-203">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="51a9d-203">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="51a9d-204">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="51a9d-204">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="51a9d-205">Ters proxy yapılandırılmış ve systemd yönetilen Kestrel ile web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-205">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="51a9d-206">Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-206">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="51a9d-207">Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-207">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="51a9d-208">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="51a9d-208">Viewing logs</span></span>

<span data-ttu-id="51a9d-209">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-209">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="51a9d-210">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="51a9d-210">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="51a9d-211">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="51a9d-211">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="51a9d-212">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.</span><span class="sxs-lookup"><span data-stu-id="51a9d-212">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="51a9d-213">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="51a9d-213">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="51a9d-214">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="51a9d-214">Enable AppArmor</span></span>

<span data-ttu-id="51a9d-215">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-215">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="51a9d-216">LSM güvenlik modüllerin farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="51a9d-216">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="51a9d-217">[AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil.</span><span class="sxs-lookup"><span data-stu-id="51a9d-217">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="51a9d-218">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="51a9d-218">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="51a9d-219">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51a9d-219">Configuring the firewall</span></span>

<span data-ttu-id="51a9d-220">Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-220">Close off all external ports that are not in use.</span></span> <span data-ttu-id="51a9d-221">Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="51a9d-221">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="51a9d-222">Doğrulayın `ufw` gereken herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="51a9d-222">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="51a9d-223">Nginx güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="51a9d-223">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="51a9d-224">Nginx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="51a9d-224">Change the Nginx response name</span></span>

<span data-ttu-id="51a9d-225">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="51a9d-225">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="51a9d-226">Seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51a9d-226">Configure options</span></span>

<span data-ttu-id="51a9d-227">Sunucu ile ek gerekli modüllerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-227">Configure the server with additional required modules.</span></span> <span data-ttu-id="51a9d-228">Bir web uygulaması güvenlik duvarı gibi kullanmayı [ModSecurity](https://www.modsecurity.org/), uygulama sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="51a9d-228">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="51a9d-229">SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51a9d-229">Configure SSL</span></span>

* <span data-ttu-id="51a9d-230">Bağlantı noktasında HTTPS trafiğini dinlemek üzere yapılandırmak `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.</span><span class="sxs-lookup"><span data-stu-id="51a9d-230">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="51a9d-231">Aşağıda gösterilen uygulamaları kullanan güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="51a9d-231">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="51a9d-232">Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-232">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="51a9d-233">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.</span><span class="sxs-lookup"><span data-stu-id="51a9d-233">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="51a9d-234">Strict Aktarım güvenlik üstbilgisi ekleme veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışıysa.</span><span class="sxs-lookup"><span data-stu-id="51a9d-234">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="51a9d-235">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="51a9d-235">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="51a9d-236">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="51a9d-236">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="51a9d-237">Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="51a9d-237">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="51a9d-238">Clickjacking öğesini gelen güvenli Nginx</span><span class="sxs-lookup"><span data-stu-id="51a9d-238">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="51a9d-239">Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="51a9d-239">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="51a9d-240">Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları.</span><span class="sxs-lookup"><span data-stu-id="51a9d-240">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="51a9d-241">Kullanım X-FRAME-site güvenli hale getirmek için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="51a9d-241">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="51a9d-242">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="51a9d-242">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="51a9d-243">Satırı ekleyin `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-243">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="51a9d-244">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="51a9d-244">MIME-type sniffing</span></span>

<span data-ttu-id="51a9d-245">Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması çoğu tarayıcılarından yanıt içerik türü, yöntemi bir kenara bırakarak engeller.</span><span class="sxs-lookup"><span data-stu-id="51a9d-245">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="51a9d-246">İle `nosniff` seçeneği, sunucunun içeriktir "text/html" diyorsa, tarayıcı, "text/html" işler.</span><span class="sxs-lookup"><span data-stu-id="51a9d-246">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="51a9d-247">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="51a9d-247">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="51a9d-248">Satırı ekleyin `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="51a9d-248">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51a9d-249">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="51a9d-249">Additional resources</span></span>

* [<span data-ttu-id="51a9d-250">Nginx: İkili sürümler: resmi Debian/Ubuntu paketleri</span><span class="sxs-lookup"><span data-stu-id="51a9d-250">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
