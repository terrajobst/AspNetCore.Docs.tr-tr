---
title: ASP.NET Core Nginx ile Linux ana bilgisayar
author: rick-anderson
description: "Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu Nginx Kurulum açıklar."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 9939e420fee41b11e709da911d4051a048e789b3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="669d2-103">ASP.NET Core Nginx ile Linux ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="669d2-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="669d2-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="669d2-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="669d2-105">Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar.</span><span class="sxs-lookup"><span data-stu-id="669d2-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="669d2-106">**Not:** Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="669d2-107">*systemd* Ubuntu 14.04 üzerinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="669d2-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="669d2-108">Bu belgenin önceki sürümünü bakın</span><span class="sxs-lookup"><span data-stu-id="669d2-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="669d2-109">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="669d2-109">This guide:</span></span>

* <span data-ttu-id="669d2-110">Var olan bir ASP.NET Core uygulamaya bir ters Ara sunucu arkasındaki yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="669d2-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="669d2-111">Kestrel web sunucusu isteklerini iletmek için ters proxy sunucuyu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="669d2-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="669d2-112">Web uygulaması başlangıçta bir arka plan programı olarak gerçekleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="669d2-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="669d2-113">Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="669d2-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="669d2-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="669d2-114">Prerequisites</span></span>

1. <span data-ttu-id="669d2-115">Sudo ayrıcalığa sahip standart kullanıcı hesabı ile bir Ubuntu 16.04 sunucuya erişimi</span><span class="sxs-lookup"><span data-stu-id="669d2-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="669d2-116">Mevcut bir ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="669d2-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="669d2-117">Uygulama kopyalama</span><span class="sxs-lookup"><span data-stu-id="669d2-117">Copy over the app</span></span>

<span data-ttu-id="669d2-118">Çalıştırma `dotnet publish` bir uygulama sunucusunda çalışabilecek kendi içinde bulunan bir dizine paketlemek için geliştirme ortamı'ndan.</span><span class="sxs-lookup"><span data-stu-id="669d2-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="669d2-119">Kopya ne olursa olsun aracını kullanarak sunucu ASP.NET Core uygulamaya kuruluşun akışına (örneğin, SCP, FTP) tümleştirir.</span><span class="sxs-lookup"><span data-stu-id="669d2-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="669d2-120">Uygulama, örneğin test edin:</span><span class="sxs-lookup"><span data-stu-id="669d2-120">Test the app, for example:</span></span>

* <span data-ttu-id="669d2-121">Komut satırından çalıştırma `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="669d2-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="669d2-122">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulama Linux üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="669d2-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="669d2-123">Ters proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="669d2-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="669d2-124">Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="669d2-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="669d2-125">Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.</span><span class="sxs-lookup"><span data-stu-id="669d2-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="669d2-126">Ters proxy sunucusu neden kullanılır?</span><span class="sxs-lookup"><span data-stu-id="669d2-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="669d2-127">Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için mükemmeldir.</span><span class="sxs-lookup"><span data-stu-id="669d2-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="669d2-128">Bununla birlikte, IIS, Apache veya Nginx gibi sunucuları olarak özellik zengin olarak web hizmet yetenekleri değil.</span><span class="sxs-lookup"><span data-stu-id="669d2-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="669d2-129">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="669d2-130">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="669d2-131">Bu kılavuzun amaçları Nginx tek bir örneğini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="669d2-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="669d2-132">HTTP sunucusu yanında aynı sunucu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="669d2-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="669d2-133">Gereksinimlerine bağlı olarak, farklı kurulum tarihte olabilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-133">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="669d2-134">İstekleri tarafından ters proxy iletilir olduğundan `ForwardedHeaders` Ara `Microsoft.AspNetCore.HttpOverrides` paket.</span><span class="sxs-lookup"><span data-stu-id="669d2-134">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="669d2-135">Bu ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="669d2-135">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="669d2-136">Ters proxy sunucusu kurma, kimlik doğrulaması ara yazılımı gereken `UseForwardedHeaders` ilk önce çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="669d2-136">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="669d2-137">Bu sıralama, kimlik doğrulaması ara yazılımı etkilenen değerleri kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="669d2-137">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="669d2-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="669d2-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="669d2-139">Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseAuthentication` veya benzer kimlik doğrulama düzeni ara:</span><span class="sxs-lookup"><span data-stu-id="669d2-139">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="669d2-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="669d2-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="669d2-141">Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseIdentity` ve `UseFacebookAuthentication` veya benzer kimlik doğrulama düzeni ara:</span><span class="sxs-lookup"><span data-stu-id="669d2-141">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="669d2-142">Nginx yükleyin</span><span class="sxs-lookup"><span data-stu-id="669d2-142">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="669d2-143">İsteğe bağlı Nginx modülleri yüklü değilse, Nginx kaynağından derleme gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-143">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="669d2-144">Kullanım `apt-get` Nginx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="669d2-144">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="669d2-145">Yükleyici Nginx arka plan programı gibi sistem başlangıcında çalışan bir sistem V init betiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="669d2-145">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="669d2-146">Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="669d2-146">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="669d2-147">Giriş sayfasında Nginx için varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="669d2-147">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="669d2-148">Nginx yapılandırın</span><span class="sxs-lookup"><span data-stu-id="669d2-148">Configure Nginx</span></span>

<span data-ttu-id="669d2-149">Ters Ara sunucu istekleri iletmek üzere ASP.NET Core uygulamamıza Nginx yapılandırmak için değiştirin `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="669d2-149">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="669d2-150">Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="669d2-150">Open it in a text editor, and replace the contents with the following:</span></span>

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="669d2-151">Bu Nginx yapılandırma dosyası gelen ortak bağlantı noktasından trafiğini `80` bağlantı noktasına `5000`.</span><span class="sxs-lookup"><span data-stu-id="669d2-151">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="669d2-152">Nginx yapılandırma kurulduktan sonra çalıştırmak `sudo nginx -t` yapılandırma dosyalarını söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="669d2-152">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="669d2-153">Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için Nginx zorla `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="669d2-153">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="669d2-154">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="669d2-154">Monitoring the app</span></span>

<span data-ttu-id="669d2-155">Yapılan isteklerini iletmek için Kurulum sunucusudur `http://<serveraddress>:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="669d2-155">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="669d2-156">Ancak, Nginx Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="669d2-156">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="669d2-157">*systemd* başlatmak ve temel web uygulaması izleme için bir hizmet dosyasını oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-157">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="669d2-158">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="669d2-158">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="669d2-159">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="669d2-159">Create the service file</span></span>

<span data-ttu-id="669d2-160">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="669d2-160">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="669d2-161">Uygulama için bir örnek hizmet dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="669d2-161">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="669d2-162">**Not:** , kullanıcı *www veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="669d2-162">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="669d2-163">**Not:** Linux büyük küçük harfe duyarlı dosya sistemine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="669d2-163">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="669d2-164">Yapılandırma dosyası için arama sonuçlarındaki "Üretim" ASPNETCORE_ENVIRONMENT ayarlanması *appsettings. Production.JSON*değil *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="669d2-164">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="669d2-165">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="669d2-165">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="669d2-166">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="669d2-166">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="669d2-167">Ters proxy yapılandırılmış ve systemd yönetilen Kestrel ile web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="669d2-167">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="669d2-168">Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-168">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="669d2-169">Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="669d2-169">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="669d2-170">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="669d2-170">Viewing logs</span></span>

<span data-ttu-id="669d2-171">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-171">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="669d2-172">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="669d2-172">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="669d2-173">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="669d2-173">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="669d2-174">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.</span><span class="sxs-lookup"><span data-stu-id="669d2-174">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="669d2-175">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="669d2-175">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="669d2-176">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="669d2-176">Enable AppArmor</span></span>

<span data-ttu-id="669d2-177">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="669d2-177">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="669d2-178">LSM güvenlik modüllerin farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="669d2-178">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="669d2-179">[AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil.</span><span class="sxs-lookup"><span data-stu-id="669d2-179">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="669d2-180">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="669d2-180">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="669d2-181">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="669d2-181">Configuring the firewall</span></span>

<span data-ttu-id="669d2-182">Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın.</span><span class="sxs-lookup"><span data-stu-id="669d2-182">Close off all external ports that are not in use.</span></span> <span data-ttu-id="669d2-183">Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="669d2-183">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="669d2-184">Doğrulayın `ufw` gereken herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="669d2-184">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="669d2-185">Nginx güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="669d2-185">Securing Nginx</span></span>

<span data-ttu-id="669d2-186">Nginx varsayılan dağıtımını SSL sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="669d2-186">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="669d2-187">Ek güvenlik özellikleri etkinleştirmek için kaynak sunucudan oluşturun.</span><span class="sxs-lookup"><span data-stu-id="669d2-187">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="669d2-188">Kaynak yükleyip derleme bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="669d2-188">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="669d2-189">Nginx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="669d2-189">Change the Nginx response name</span></span>

<span data-ttu-id="669d2-190">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="669d2-190">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="669d2-191">Seçenekleri yapılandırmak ve derleme</span><span class="sxs-lookup"><span data-stu-id="669d2-191">Configure the options and build</span></span>

<span data-ttu-id="669d2-192">Normal ifadeler için PCRE kitaplığı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="669d2-192">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="669d2-193">Normal ifadeler konumu yönergesinde ngx_http_rewrite_module için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="669d2-193">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="669d2-194">Http_ssl_module HTTPS protokolü desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="669d2-194">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="669d2-195">Bir web uygulaması güvenlik duvarı gibi kullanmayı *ModSecurity* uygulama sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="669d2-195">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="669d2-196">SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="669d2-196">Configure SSL</span></span>

* <span data-ttu-id="669d2-197">Bağlantı noktasında HTTPS trafiğini dinlemek üzere yapılandırmak `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.</span><span class="sxs-lookup"><span data-stu-id="669d2-197">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="669d2-198">Aşağıda gösterilen uygulamaları kullanan güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="669d2-198">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="669d2-199">Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="669d2-199">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="669d2-200">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.</span><span class="sxs-lookup"><span data-stu-id="669d2-200">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="669d2-201">Strict Aktarım güvenlik üstbilgisi ekleme veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışıysa.</span><span class="sxs-lookup"><span data-stu-id="669d2-201">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="669d2-202">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="669d2-202">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="669d2-203">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="669d2-203">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="669d2-204">Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="669d2-204">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="669d2-205">Clickjacking öğesini gelen güvenli Nginx</span><span class="sxs-lookup"><span data-stu-id="669d2-205">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="669d2-206">Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="669d2-206">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="669d2-207">Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları.</span><span class="sxs-lookup"><span data-stu-id="669d2-207">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="669d2-208">Kullanım X-FRAME-site güvenli hale getirmek için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="669d2-208">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="669d2-209">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="669d2-209">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="669d2-210">Satırı ekleyin `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="669d2-210">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="669d2-211">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="669d2-211">MIME-type sniffing</span></span>

<span data-ttu-id="669d2-212">Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması çoğu tarayıcılarından yanıt içerik türü, yöntemi bir kenara bırakarak engeller.</span><span class="sxs-lookup"><span data-stu-id="669d2-212">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="669d2-213">İle `nosniff` seçeneği, sunucunun içeriktir "text/html" diyorsa, tarayıcı, "text/html" işler.</span><span class="sxs-lookup"><span data-stu-id="669d2-213">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="669d2-214">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="669d2-214">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="669d2-215">Satırı ekleyin `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin, sonra Nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="669d2-215">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
