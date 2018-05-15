---
title: ASP.NET Core Nginx ile Linux ana bilgisayar
author: rick-anderson
description: Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu Nginx Kurulum açıklar.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: fe772203e5e3fceb7489e0a5866f60ea914b7329
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="749d5-103">ASP.NET Core Nginx ile Linux ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="749d5-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="749d5-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="749d5-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="749d5-105">Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar.</span><span class="sxs-lookup"><span data-stu-id="749d5-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="749d5-106">Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="749d5-107">*systemd* Ubuntu 14.04 üzerinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="749d5-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="749d5-108">[Bu belgenin önceki sürümünü görmek](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="749d5-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="749d5-109">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="749d5-109">This guide:</span></span>

* <span data-ttu-id="749d5-110">Var olan bir ASP.NET Core uygulamaya bir ters Ara sunucu arkasındaki yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="749d5-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="749d5-111">Kestrel web sunucusu isteklerini iletmek için ters proxy sunucuyu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="749d5-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="749d5-112">Web uygulaması başlangıçta bir arka plan programı olarak gerçekleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="749d5-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="749d5-113">Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="749d5-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="749d5-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="749d5-114">Prerequisites</span></span>

1. <span data-ttu-id="749d5-115">Sudo ayrıcalığa sahip standart kullanıcı hesabı ile bir Ubuntu 16.04 sunucuya erişimi</span><span class="sxs-lookup"><span data-stu-id="749d5-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="749d5-116">Mevcut bir ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="749d5-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="749d5-117">Uygulama kopyalama</span><span class="sxs-lookup"><span data-stu-id="749d5-117">Copy over the app</span></span>

<span data-ttu-id="749d5-118">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir uygulama sunucusunda çalışabilecek kendi içinde bulunan bir dizine paketlemek için geliştirme ortamı'ndan.</span><span class="sxs-lookup"><span data-stu-id="749d5-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="749d5-119">Kopya ne olursa olsun aracını kullanarak sunucu ASP.NET Core uygulamaya kuruluşun akışına (örneğin, SCP, FTP) tümleştirir.</span><span class="sxs-lookup"><span data-stu-id="749d5-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="749d5-120">Uygulama, örneğin test edin:</span><span class="sxs-lookup"><span data-stu-id="749d5-120">Test the app, for example:</span></span>

* <span data-ttu-id="749d5-121">Komut satırından çalıştırma `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="749d5-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="749d5-122">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulama Linux üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="749d5-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="749d5-123">Ters proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="749d5-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="749d5-124">Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="749d5-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="749d5-125">Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.</span><span class="sxs-lookup"><span data-stu-id="749d5-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="749d5-126">Ters proxy sunucusu neden kullanılır?</span><span class="sxs-lookup"><span data-stu-id="749d5-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="749d5-127">Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için mükemmeldir.</span><span class="sxs-lookup"><span data-stu-id="749d5-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="749d5-128">Bununla birlikte, IIS, Apache veya Nginx gibi sunucuları olarak özellik zengin olarak web hizmet yetenekleri değil.</span><span class="sxs-lookup"><span data-stu-id="749d5-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="749d5-129">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="749d5-130">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="749d5-131">Bu kılavuzun amaçları Nginx tek bir örneğini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="749d5-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="749d5-132">HTTP sunucusu yanında aynı sunucu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="749d5-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="749d5-133">Gereksinimlerine bağlı olarak, farklı kurulum seçmiş olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="749d5-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="749d5-134">İstekleri tarafından ters proxy iletilir çünkü iletilen üstbilgileri Ara kullanmak [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="749d5-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="749d5-135">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="749d5-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="749d5-136">Kimlik doğrulaması ara yazılımı herhangi bir türde kullanırken, iletilen üstbilgileri Ara ilk çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="749d5-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="749d5-137">Bu sıralama, kimlik doğrulaması ara yazılımı üstbilgi değerlerini kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="749d5-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="749d5-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="749d5-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="749d5-139">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulama düzeni ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="749d5-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="749d5-140">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="749d5-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="749d5-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="749d5-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="749d5-142">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer kimlik doğrulama şeması Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="749d5-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="749d5-143">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="749d5-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="749d5-144">Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan üstbilgiler `None`.</span><span class="sxs-lookup"><span data-stu-id="749d5-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="749d5-145">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="749d5-146">Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="749d5-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="749d5-147">Nginx yükleyin</span><span class="sxs-lookup"><span data-stu-id="749d5-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="749d5-148">İsteğe bağlı Nginx modülleri yüklü değilse, Nginx kaynağından derleme gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="749d5-149">Kullanım `apt-get` Nginx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="749d5-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="749d5-150">Yükleyici Nginx arka plan programı gibi sistem başlangıcında çalışan bir sistem V init betiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="749d5-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="749d5-151">Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="749d5-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="749d5-152">Giriş sayfasında Nginx için varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="749d5-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="749d5-153">Nginx yapılandırın</span><span class="sxs-lookup"><span data-stu-id="749d5-153">Configure Nginx</span></span>

<span data-ttu-id="749d5-154">ASP.NET Core uygulamanıza istekleri iletmek için ters Ara sunucu Nginx yapılandırmak için değiştirin */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="749d5-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="749d5-155">Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="749d5-155">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="749d5-156">Durumlarda `server_name` eşleşirse, Nginx varsayılan sunucu kullanır.</span><span class="sxs-lookup"><span data-stu-id="749d5-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="749d5-157">Varsayılan sunucu tanımlanmışsa ilk yapılandırma dosyasındaki varsayılan sunucu sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="749d5-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="749d5-158">En iyi uygulama, yapılandırma dosyanızda 444 bir durum kodunu döndüren bir belirli varsayılan sunucu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="749d5-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="749d5-159">Varsayılan sunucu yapılandırma örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="749d5-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="749d5-160">Önceki yapılandırma dosyası ve varsayılan sunucusuyla, Nginx ana bilgisayar üstbilgisi ile 80 numaralı bağlantı noktasında genel trafiği kabul `example.com` veya `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="749d5-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="749d5-161">Bu konaklar eşleşmeyen istekleri için Kestrel iletilen olmaz.</span><span class="sxs-lookup"><span data-stu-id="749d5-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="749d5-162">Nginx Kestrel eşleşen isteklerini iletir `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="749d5-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="749d5-163">Bkz: [nasıl nginx bir isteği işler](https://nginx.org/docs/http/request_processing.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="749d5-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="749d5-164">Uygun belirtmek için hata [sunucu_adı yönergesi](https://nginx.org/docs/http/server_names.html) güvenlik açıkları, uygulamanızın kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="749d5-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="749d5-165">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanı denetlemek, bu güvenlik riski değil (tersine `*.com`, açık olduğu).</span><span class="sxs-lookup"><span data-stu-id="749d5-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="749d5-166">Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="749d5-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="749d5-167">Nginx yapılandırma kurulduktan sonra çalıştırmak `sudo nginx -t` yapılandırma dosyalarını söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="749d5-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="749d5-168">Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için Nginx zorla `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="749d5-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="749d5-169">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="749d5-169">Monitoring the app</span></span>

<span data-ttu-id="749d5-170">Yapılan isteklerini iletmek için Kurulum sunucusudur `http://<serveraddress>:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="749d5-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="749d5-171">Ancak, Nginx Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="749d5-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="749d5-172">*systemd* başlatmak ve temel web uygulaması izleme için bir hizmet dosyasını oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="749d5-173">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="749d5-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="749d5-174">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="749d5-174">Create the service file</span></span>

<span data-ttu-id="749d5-175">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="749d5-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="749d5-176">Uygulama için bir örnek hizmet dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="749d5-176">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="749d5-177">**Not:** , kullanıcı *www veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="749d5-177">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="749d5-178">**Not:** Linux büyük küçük harfe duyarlı dosya sistemine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="749d5-178">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="749d5-179">Yapılandırma dosyası için arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="749d5-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="749d5-180">Ortam değişkenleri okumak yapılandırma sağlayıcısı için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçış uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="749d5-180">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="749d5-181">Yapılandırma dosyasında kullanmak için düzgün bir şekilde Atlanan değeri oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="749d5-181">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="749d5-182">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="749d5-182">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="749d5-183">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="749d5-183">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="749d5-184">Ters proxy yapılandırılmış ve systemd yönetilen Kestrel ile web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="749d5-184">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="749d5-185">Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-185">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="749d5-186">Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="749d5-186">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="749d5-187">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="749d5-187">Viewing logs</span></span>

<span data-ttu-id="749d5-188">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-188">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="749d5-189">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="749d5-189">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="749d5-190">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="749d5-190">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="749d5-191">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.</span><span class="sxs-lookup"><span data-stu-id="749d5-191">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="749d5-192">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="749d5-192">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="749d5-193">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="749d5-193">Enable AppArmor</span></span>

<span data-ttu-id="749d5-194">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="749d5-194">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="749d5-195">LSM güvenlik modüllerin farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="749d5-195">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="749d5-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil.</span><span class="sxs-lookup"><span data-stu-id="749d5-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="749d5-197">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="749d5-197">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="749d5-198">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="749d5-198">Configuring the firewall</span></span>

<span data-ttu-id="749d5-199">Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın.</span><span class="sxs-lookup"><span data-stu-id="749d5-199">Close off all external ports that are not in use.</span></span> <span data-ttu-id="749d5-200">Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="749d5-200">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="749d5-201">Doğrulayın `ufw` gereken herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="749d5-201">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="749d5-202">Nginx güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="749d5-202">Securing Nginx</span></span>

<span data-ttu-id="749d5-203">Nginx varsayılan dağıtımını SSL sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="749d5-203">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="749d5-204">Ek güvenlik özellikleri etkinleştirmek için kaynak sunucudan oluşturun.</span><span class="sxs-lookup"><span data-stu-id="749d5-204">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="749d5-205">Kaynak yükleyip derleme bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="749d5-205">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="749d5-206">Nginx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="749d5-206">Change the Nginx response name</span></span>

<span data-ttu-id="749d5-207">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="749d5-207">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="749d5-208">Seçenekleri yapılandırmak ve derleme</span><span class="sxs-lookup"><span data-stu-id="749d5-208">Configure the options and build</span></span>

<span data-ttu-id="749d5-209">Normal ifadeler için PCRE kitaplığı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="749d5-209">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="749d5-210">Normal ifadeler konumu yönergesinde ngx_http_rewrite_module için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="749d5-210">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="749d5-211">Http_ssl_module HTTPS protokolü desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="749d5-211">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="749d5-212">Bir web uygulaması güvenlik duvarı gibi kullanmayı *ModSecurity* uygulama sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="749d5-212">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="749d5-213">SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="749d5-213">Configure SSL</span></span>

* <span data-ttu-id="749d5-214">Bağlantı noktasında HTTPS trafiğini dinlemek üzere yapılandırmak `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.</span><span class="sxs-lookup"><span data-stu-id="749d5-214">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="749d5-215">Aşağıda gösterilen uygulamaları kullanan güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="749d5-215">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="749d5-216">Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="749d5-216">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="749d5-217">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.</span><span class="sxs-lookup"><span data-stu-id="749d5-217">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="749d5-218">Strict Aktarım güvenlik üstbilgisi ekleme veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışıysa.</span><span class="sxs-lookup"><span data-stu-id="749d5-218">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="749d5-219">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="749d5-219">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="749d5-220">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="749d5-220">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="749d5-221">Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="749d5-221">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="749d5-222">Clickjacking öğesini gelen güvenli Nginx</span><span class="sxs-lookup"><span data-stu-id="749d5-222">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="749d5-223">Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="749d5-223">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="749d5-224">Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları.</span><span class="sxs-lookup"><span data-stu-id="749d5-224">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="749d5-225">Kullanım X-FRAME-site güvenli hale getirmek için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="749d5-225">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="749d5-226">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="749d5-226">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="749d5-227">Satırı ekleyin `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="749d5-227">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="749d5-228">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="749d5-228">MIME-type sniffing</span></span>

<span data-ttu-id="749d5-229">Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması çoğu tarayıcılarından yanıt içerik türü, yöntemi bir kenara bırakarak engeller.</span><span class="sxs-lookup"><span data-stu-id="749d5-229">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="749d5-230">İle `nosniff` seçeneği, sunucunun içeriktir "text/html" diyorsa, tarayıcı, "text/html" işler.</span><span class="sxs-lookup"><span data-stu-id="749d5-230">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="749d5-231">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="749d5-231">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="749d5-232">Satırı ekleyin `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="749d5-232">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
