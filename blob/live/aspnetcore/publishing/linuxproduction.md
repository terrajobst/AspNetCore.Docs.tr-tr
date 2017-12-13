---
title: ASP.NET Core Nginx ile Linux ana bilgisayar
description: "Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulama HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu Nginx Kurulum açıklar."
keywords: ASP.NET Core, Linux, nginx, Ubuntu, Ters Proxy
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 01768263fe82dc75a7da0e113b1850c8d788bfd3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="f5c2e-104">ASP.NET Core Nginx ile Linux için bir barındırma ortamını ayarlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="f5c2e-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="f5c2e-105">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="f5c2e-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="f5c2e-106">Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="f5c2e-107">**Not:** Kestrel işlem izleme için bir çözüm olarak supervisord Ubuntu 14.04 için önerilir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="f5c2e-108">systemd Ubuntu 14.04 üzerinde kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="f5c2e-109">Bu belgenin önceki sürümünü bakın</span><span class="sxs-lookup"><span data-stu-id="f5c2e-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="f5c2e-110">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-110">This guide:</span></span>

* <span data-ttu-id="f5c2e-111">Var olan bir ASP.NET Core uygulamasını bir ters Ara sunucu arkasındaki yerleştirir</span><span class="sxs-lookup"><span data-stu-id="f5c2e-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="f5c2e-112">Ters proxy sunucuyu isteklerini Kestrel web sunucusuna iletmek için ayarlar</span><span class="sxs-lookup"><span data-stu-id="f5c2e-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="f5c2e-113">Web uygulaması bir arka plan programı Başlangıç çalışır sağlar</span><span class="sxs-lookup"><span data-stu-id="f5c2e-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="f5c2e-114">Web uygulaması yeniden yardımcı olmak için bir işlem yönetim aracı yapılandırır</span><span class="sxs-lookup"><span data-stu-id="f5c2e-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5c2e-115">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f5c2e-115">Prerequisites</span></span>

1. <span data-ttu-id="f5c2e-116">Sudo ayrıcalığa sahip standart kullanıcı hesabı ile bir Ubuntu 16.04 sunucuya erişimi</span><span class="sxs-lookup"><span data-stu-id="f5c2e-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="f5c2e-117">Mevcut bir ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="f5c2e-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="f5c2e-118">Uygulamanızı kopyalama</span><span class="sxs-lookup"><span data-stu-id="f5c2e-118">Copy over your app</span></span>

<span data-ttu-id="f5c2e-119">Çalıştırma `dotnet publish` bir uygulama sunucusunda çalışabilecek kendi içinde bulunan bir dizine paketlemek için geliştirme ortamı'ndan.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="f5c2e-120">ASP.NET Core uygulama ne olursa olsun Aracı (SCP, FTP, vb.) iş akışınıza tümleştirir kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="f5c2e-121">Uygulama, örneğin test edin:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-121">Test the app, for example:</span></span>
 - <span data-ttu-id="f5c2e-122">Komut satırından çalıştırma`dotnet yourapp.dll`</span><span class="sxs-lookup"><span data-stu-id="f5c2e-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="f5c2e-123">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulama Linux üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
<span data-ttu-id="f5c2e-124">**Not:** kullanım [Yeoman](xref:client-side/yeoman) yeni bir proje için yeni bir ASP.NET Core uygulaması oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-124">**Note:** Use [Yeoman](xref:client-side/yeoman) to create a new ASP.NET Core app for a new project.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="f5c2e-125">Ters proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f5c2e-125">Configure a reverse proxy server</span></span>

<span data-ttu-id="f5c2e-126">Ters proxy dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-126">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="f5c2e-127">Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-127">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="f5c2e-128">Ters proxy sunucusu neden kullanılır?</span><span class="sxs-lookup"><span data-stu-id="f5c2e-128">Why use a reverse proxy server?</span></span>

<span data-ttu-id="f5c2e-129">Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için harikadır; Bununla birlikte, IIS, Apache veya Nginx gibi sunucuları olarak özellik zengin olarak web hizmet bölümleri değil.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-129">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="f5c2e-130">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-130">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="f5c2e-131">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-131">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="f5c2e-132">Bu kılavuzun amaçları Nginx tek bir örneğini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-132">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="f5c2e-133">HTTP sunucusu yanında aynı sunucu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-133">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="f5c2e-134">Gereksinimlerinize bağlı olarak, farklı kurulum seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-134">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="f5c2e-135">İstekleri tarafından ters proxy iletilir olduğundan `ForwardedHeaders` Ara `Microsoft.AspNetCore.HttpOverrides` paket.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-135">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="f5c2e-136">Bu ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-136">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="f5c2e-137">Ters proxy sunucusu kurma, kimlik doğrulaması ara yazılımı gereken `UseForwardedHeaders` ilk önce çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-137">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="f5c2e-138">Bu sıralama, kimlik doğrulaması ara yazılımı etkilenen değerleri kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-138">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f5c2e-139">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="f5c2e-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f5c2e-140">Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseAuthentication` veya benzer kimlik doğrulama düzeni ara:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f5c2e-141">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="f5c2e-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f5c2e-142">Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseIdentity` ve `UseFacebookAuthentication` veya benzer kimlik doğrulama düzeni ara:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-142">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="f5c2e-143">Nginx yükleyin</span><span class="sxs-lookup"><span data-stu-id="f5c2e-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="f5c2e-144">İsteğe bağlı Nginx modülleri yüklemeyi planlıyorsanız, Nginx kaynağından oluşturmak için gerekli.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-144">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="f5c2e-145">Kullanım `apt-get` Nginx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="f5c2e-146">Yükleyici Nginx arka plan programı gibi sistem başlangıcında çalışan bir sistem V init betiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="f5c2e-147">Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="f5c2e-148">Giriş sayfasında Nginx için varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="f5c2e-149">Nginx yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f5c2e-149">Configure Nginx</span></span>

<span data-ttu-id="f5c2e-150">ASP.NET Core uygulamamız için istekleri iletmek için ters Ara sunucu Nginx yapılandırmak için değiştirin `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-150">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="f5c2e-151">Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-151">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="f5c2e-152">Bu Nginx yapılandırma dosyası gelen ortak bağlantı noktasından trafiğini `80` bağlantı noktasına `5000`.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-152">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="f5c2e-153">Tamamladıktan sonra değişiklik yapma Nginx yapılandırmanızı, çalıştırabilirsiniz `sudo nginx -t` yapılandırma dosyalarınızın söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-153">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="f5c2e-154">Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için Nginx sorabileceğiniz `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-154">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="f5c2e-155">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="f5c2e-155">Monitoring our application</span></span>

<span data-ttu-id="f5c2e-156">Nginx olan şimdi yapılan isteklerini iletmek için kurulumu `http://yourhost:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-156">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="f5c2e-157">Ancak, Nginx Kestrel işlemini yönetmek üzere ayarlanmış değil.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-157">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="f5c2e-158">Kullanabileceğiniz *systemd* ve Başlat ve temel web uygulaması izlemek için bir hizmet dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-158">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="f5c2e-159">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="f5c2e-160">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="f5c2e-160">Create the service file</span></span>

<span data-ttu-id="f5c2e-161">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="f5c2e-162">Uygulamamız için örnek bir hizmet dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-162">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

<span data-ttu-id="f5c2e-163">**Not:** , kullanıcı *www veri* kullanılmaz yapılandırmanıza göre burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-163">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="f5c2e-164">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-164">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="f5c2e-165">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-165">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="f5c2e-166">Web uygulaması yapılandırılmış ters proxy ve systemd yönetilen Kestrel ile tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-166">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="f5c2e-167">Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-167">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="f5c2e-168">Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-168">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="f5c2e-169">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="f5c2e-169">Viewing logs</span></span>

<span data-ttu-id="f5c2e-170">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-170">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="f5c2e-171">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-171">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="f5c2e-172">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-172">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="f5c2e-173">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-173">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="f5c2e-174">Uygulamamız güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="f5c2e-174">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="f5c2e-175">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="f5c2e-175">Enable AppArmor</span></span>

<span data-ttu-id="f5c2e-176">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-176">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="f5c2e-177">LSM güvenlik modüllerin farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-177">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="f5c2e-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="f5c2e-179">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-179">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="f5c2e-180">Bizim güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f5c2e-180">Configuring our firewall</span></span>

<span data-ttu-id="f5c2e-181">Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-181">Close off all external ports that are not in use.</span></span> <span data-ttu-id="f5c2e-182">Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-182">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="f5c2e-183">Doğrulayın `ufw` gereksinim duyduğunuz herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-183">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="f5c2e-184">Nginx güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="f5c2e-184">Securing Nginx</span></span>

<span data-ttu-id="f5c2e-185">Nginx varsayılan dağıtımını SSL sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-185">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="f5c2e-186">Ek güvenlik özellikleri etkinleştirmek için kaynak sunucudan oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-186">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="f5c2e-187">Kaynak yükleyip derleme bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="f5c2e-187">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="f5c2e-188">Nginx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="f5c2e-188">Change the Nginx response name</span></span>

<span data-ttu-id="f5c2e-189">Düzen *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-189">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="f5c2e-190">Seçenekleri yapılandırmak ve derleme</span><span class="sxs-lookup"><span data-stu-id="f5c2e-190">Configure the options and build</span></span>

<span data-ttu-id="f5c2e-191">Normal ifadeler için PCRE kitaplığı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-191">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="f5c2e-192">Normal ifadeler konumu yönergesinde ngx_http_rewrite_module için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-192">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="f5c2e-193">Http_ssl_module HTTPS protokolü desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-193">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="f5c2e-194">Web uygulaması güvenlik duvarı gibi kullanmayı *ModSecurity* uygulamanız sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-194">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="f5c2e-195">SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f5c2e-195">Configure SSL</span></span>

* <span data-ttu-id="f5c2e-196">Bağlantı noktasında HTTPS trafiğini dinlemek için sunucu yapılandırma `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-196">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="f5c2e-197">Aşağıda gösterilen uygulamaları kullanan güvenlik sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-197">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="f5c2e-198">Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-198">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="f5c2e-199">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-199">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="f5c2e-200">Strict Aktarım güvenlik üstbilgisi eklemeyin veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışı planlıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-200">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="f5c2e-201">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-201">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="f5c2e-202">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-202">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="f5c2e-203">Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-203">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="f5c2e-204">Clickjacking öğesini gelen güvenli Nginx</span><span class="sxs-lookup"><span data-stu-id="f5c2e-204">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="f5c2e-205">Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-205">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="f5c2e-206">Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-206">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="f5c2e-207">Kullanım X-FRAME-sitenizin güvenliğini sağlamak için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-207">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="f5c2e-208">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-208">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="f5c2e-209">Satırı ekleyin `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-209">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="f5c2e-210">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="f5c2e-210">MIME-type sniffing</span></span>

<span data-ttu-id="f5c2e-211">Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması çoğu tarayıcılarından yanıt içerik türü, yöntemi bir kenara bırakarak engeller.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-211">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="f5c2e-212">İle `nosniff` seçeneği, sunucunun içeriktir "text/html" diyorsa, tarayıcı, "text/html" işler.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-212">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="f5c2e-213">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-213">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="f5c2e-214">Satırı ekleyin `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-214">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
