---
title: Ana bilgisayar ASP.NET Core nginx ile Linux
description: "Ubuntu Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması HTTP trafiği iletmek için 16.04 üzerinde ters Ara sunucu nginx Kurulum açıklar."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 08/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: bad84b8c68bd0bc63bcd125e1873bc99a2ed2afd
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="80cc6-103">Ana bilgisayar ASP.NET Core nginx ile Linux</span><span class="sxs-lookup"><span data-stu-id="80cc6-103">Host ASP.NET Core on Linux with nginx</span></span>

<span data-ttu-id="80cc6-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="80cc6-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="80cc6-105">Bu kılavuz bir Ubuntu 16.04 sunucusunda üretime hazır ASP.NET Core ortamını ayarlama açıklar.</span><span class="sxs-lookup"><span data-stu-id="80cc6-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="80cc6-106">**Not:** Ubuntu 14.04 için *supervisord* Kestrel işlem izleme için bir çözüm olarak önerilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="80cc6-107">*systemd* Ubuntu 14.04 üzerinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="80cc6-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="80cc6-108">Bu belgenin önceki sürümünü bakın</span><span class="sxs-lookup"><span data-stu-id="80cc6-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="80cc6-109">Bu Kılavuzu:</span><span class="sxs-lookup"><span data-stu-id="80cc6-109">This guide:</span></span>

* <span data-ttu-id="80cc6-110">Var olan bir ASP.NET Core uygulamaya bir ters Ara sunucu arkasındaki yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="80cc6-111">Kestrel web sunucusu isteklerini iletmek için ters proxy sunucuyu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="80cc6-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="80cc6-112">Web uygulaması başlangıçta bir arka plan programı olarak gerçekleştirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="80cc6-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="80cc6-113">Web uygulaması yeniden yardımcı olmak için bir işlem yönetimini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="80cc6-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80cc6-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="80cc6-114">Prerequisites</span></span>

1. <span data-ttu-id="80cc6-115">Sudo ayrıcalığa sahip standart kullanıcı hesabı ile bir Ubuntu 16.04 sunucuya erişimi</span><span class="sxs-lookup"><span data-stu-id="80cc6-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="80cc6-116">Mevcut bir ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="80cc6-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="80cc6-117">Uygulama kopyalama</span><span class="sxs-lookup"><span data-stu-id="80cc6-117">Copy over the app</span></span>

<span data-ttu-id="80cc6-118">Çalıştırma `dotnet publish` bir uygulama sunucusunda çalışabilecek kendi içinde bulunan bir dizine paketlemek için geliştirme ortamı'ndan.</span><span class="sxs-lookup"><span data-stu-id="80cc6-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="80cc6-119">Kopya ne olursa olsun aracını kullanarak sunucu ASP.NET Core uygulamaya kuruluşun akışına (örneğin, SCP, FTP) tümleştirir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="80cc6-120">Uygulama, örneğin test edin:</span><span class="sxs-lookup"><span data-stu-id="80cc6-120">Test the app, for example:</span></span>

* <span data-ttu-id="80cc6-121">Komut satırından çalıştırma `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="80cc6-122">Bir tarayıcıda gidin `http://<serveraddress>:<port>` uygulama Linux üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="80cc6-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="80cc6-123">Ters proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="80cc6-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="80cc6-124">Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="80cc6-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="80cc6-125">Ters proxy HTTP isteği sonlandırır ve ASP.NET Core uygulamaya iletir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="80cc6-126">Ters proxy sunucusu neden kullanılır?</span><span class="sxs-lookup"><span data-stu-id="80cc6-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="80cc6-127">Kestrel, dinamik içerik ASP.NET çekirdek hizmet vermek için harikadır; Bununla birlikte, IIS, Apache veya nginx gibi sunucuları olarak özellik zengin olarak web hizmet bölümleri değil.</span><span class="sxs-lookup"><span data-stu-id="80cc6-127">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or nginx.</span></span> <span data-ttu-id="80cc6-128">Ters proxy sunucusu, statik içerik sunan, istekleri önbelleğe alma, istekleri ve HTTP sunucusundan SSL sonlandırma sıkıştırma gibi iş boşaltabilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-128">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="80cc6-129">Ters proxy sunucusu adanmış bir makinede bulunabilir veya bir HTTP sunucusu dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-129">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="80cc6-130">Bu kılavuzun amaçları nginx tek bir örneğini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="80cc6-130">For the purposes of this guide, a single instance of nginx is used.</span></span> <span data-ttu-id="80cc6-131">HTTP sunucusu yanında aynı sunucu üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="80cc6-131">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="80cc6-132">Gereksinimlerine bağlı olarak, farklı kurulum tarihte olabilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-132">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="80cc6-133">İstekleri tarafından ters proxy iletilir olduğundan `ForwardedHeaders` Ara `Microsoft.AspNetCore.HttpOverrides` paket.</span><span class="sxs-lookup"><span data-stu-id="80cc6-133">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="80cc6-134">Bu ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="80cc6-134">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="80cc6-135">Ters proxy sunucusu kurma, kimlik doğrulaması ara yazılımı gereken `UseForwardedHeaders` ilk önce çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="80cc6-135">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="80cc6-136">Bu sıralama, kimlik doğrulaması ara yazılımı etkilenen değerleri kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="80cc6-136">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="80cc6-137">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="80cc6-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="80cc6-138">Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseAuthentication` veya benzer kimlik doğrulama düzeni ara:</span><span class="sxs-lookup"><span data-stu-id="80cc6-138">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="80cc6-139">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="80cc6-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="80cc6-140">Çağırma `UseForwardedHeaders` yöntemi (içinde `Configure` yöntemi *haline*) çağırmadan önce `UseIdentity` ve `UseFacebookAuthentication` veya benzer kimlik doğrulama düzeni ara:</span><span class="sxs-lookup"><span data-stu-id="80cc6-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="80cc6-141">Nginx yükleyin</span><span class="sxs-lookup"><span data-stu-id="80cc6-141">Install nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="80cc6-142">İsteğe bağlı nginx modülleri yüklü değilse, nginx kaynağından derleme gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-142">If optional nginx modules will be installed, building nginx from source might be required.</span></span>

<span data-ttu-id="80cc6-143">Kullanım `apt-get` nginx yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="80cc6-143">Use `apt-get` to install nginx.</span></span> <span data-ttu-id="80cc6-144">Yükleyici nginx arka plan programı gibi sistem başlangıcında çalışan bir sistem V init betiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="80cc6-144">The installer creates a System V init script that runs nginx as daemon on system startup.</span></span> <span data-ttu-id="80cc6-145">Nginx ilk kez yüklendiğinden bu yana, açıkça başlatılsın çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="80cc6-145">Since nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="80cc6-146">Giriş sayfasında nginx için varsayılan bir tarayıcı görüntüler doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="80cc6-146">Verify a browser displays the default landing page for nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="80cc6-147">Nginx yapılandırın</span><span class="sxs-lookup"><span data-stu-id="80cc6-147">Configure nginx</span></span>

<span data-ttu-id="80cc6-148">Ters Ara sunucu istekleri iletmek üzere ASP.NET Core uygulamamıza nginx yapılandırmak için değiştirin `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-148">To configure nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="80cc6-149">Bir metin düzenleyicisinde açın ve içeriği aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="80cc6-149">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
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

<span data-ttu-id="80cc6-150">Bu nginx yapılandırma dosyası gelen ortak bağlantı noktasından trafiğini `80` bağlantı noktasına `5000`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-150">This nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="80cc6-151">Nginx yapılandırma kurulduktan sonra çalıştırmak `sudo nginx -t` yapılandırma dosyalarını söz dizimini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="80cc6-151">Once the nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="80cc6-152">Yapılandırma dosyası sınaması başarılı olursa, çalıştırarak değişiklikleri almak için nginx zorla `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-152">If the configuration file test is successful, force nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="80cc6-153">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="80cc6-153">Monitoring the app</span></span>

<span data-ttu-id="80cc6-154">Yapılan isteklerini iletmek için Kurulum sunucusudur `http://<serveraddress>:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-154">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="80cc6-155">Ancak, nginx Kestrel işlemini yönetmek üzere ayarlanmış değil.</span><span class="sxs-lookup"><span data-stu-id="80cc6-155">However, nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="80cc6-156">*systemd* başlatmak ve temel web uygulaması izleme için bir hizmet dosyasını oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-156">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="80cc6-157">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-157">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="80cc6-158">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="80cc6-158">Create the service file</span></span>

<span data-ttu-id="80cc6-159">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="80cc6-159">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="80cc6-160">Uygulama için bir örnek hizmet dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="80cc6-160">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="80cc6-161">**Not:** , kullanıcı *www veri* kullanılmaz yapılandırma tarafından burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="80cc6-161">**Note:** If the user *www-data* is not used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="80cc6-162">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="80cc6-162">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="80cc6-163">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="80cc6-163">Start the service and verify that it is running.</span></span>

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

<span data-ttu-id="80cc6-164">Ters proxy yapılandırılmış ve systemd yönetilen Kestrel ile web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-164">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="80cc6-165">Ayrıca, engelliyor olabilir herhangi bir güvenlik duvarını engelleme uzak bir makineden de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-165">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="80cc6-166">Yanıt Üstbilgileri incelemek `Server` üstbilgisi tarafından Kestrel sunulmasını ASP.NET Core uygulama gösterir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-166">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="80cc6-167">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="80cc6-167">Viewing logs</span></span>

<span data-ttu-id="80cc6-168">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir `systemd`, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-168">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="80cc6-169">Ancak, bu günlük tüm hizmetleri ve işlemleri tarafından yönetilen tüm girişleri içerir `systemd`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-169">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="80cc6-170">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="80cc6-170">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="80cc6-171">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.</span><span class="sxs-lookup"><span data-stu-id="80cc6-171">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="80cc6-172">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="80cc6-172">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="80cc6-173">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="80cc6-173">Enable AppArmor</span></span>

<span data-ttu-id="80cc6-174">Linux güvenlik modülleri (LSM) itibaren Linux 2.6 Linux çekirdek parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-174">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="80cc6-175">LSM güvenlik modüllerin farklı uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="80cc6-175">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="80cc6-176">[AppArmor](https://wiki.ubuntu.com/AppArmor) kaynakları sınırlı sayıda programına sınırlandırma sağlayan bir zorunlu erişim denetimi sisteminde uygulayan bir LSM değil.</span><span class="sxs-lookup"><span data-stu-id="80cc6-176">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="80cc6-177">AppArmor etkin olduğundan ve doğru yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="80cc6-177">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="80cc6-178">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="80cc6-178">Configuring the firewall</span></span>

<span data-ttu-id="80cc6-179">Kullanılmayan tüm dış bağlantı noktalarını kapatmak kapatın.</span><span class="sxs-lookup"><span data-stu-id="80cc6-179">Close off all external ports that are not in use.</span></span> <span data-ttu-id="80cc6-180">Eylemlerin Güvenlik Duvarı (ufw) sağlayan bir ön uç için `iptables` güvenlik duvarı yapılandırması için bir komut satırı arabirimi sağlayarak.</span><span class="sxs-lookup"><span data-stu-id="80cc6-180">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="80cc6-181">Doğrulayın `ufw` gereken herhangi bir bağlantı trafiğine izin verecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="80cc6-181">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="80cc6-182">Nginx güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="80cc6-182">Securing nginx</span></span>

<span data-ttu-id="80cc6-183">Nginx varsayılan dağıtımını SSL sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="80cc6-183">The default distribution of nginx doesn't enable SSL.</span></span> <span data-ttu-id="80cc6-184">Ek güvenlik özellikleri etkinleştirmek için kaynak sunucudan oluşturun.</span><span class="sxs-lookup"><span data-stu-id="80cc6-184">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="80cc6-185">Kaynak yükleyip derleme bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="80cc6-185">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="80cc6-186">Nginx yanıt adını değiştirin</span><span class="sxs-lookup"><span data-stu-id="80cc6-186">Change the nginx response name</span></span>

<span data-ttu-id="80cc6-187">Düzen *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="80cc6-187">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="80cc6-188">Seçenekleri yapılandırmak ve derleme</span><span class="sxs-lookup"><span data-stu-id="80cc6-188">Configure the options and build</span></span>

<span data-ttu-id="80cc6-189">Normal ifadeler için PCRE kitaplığı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-189">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="80cc6-190">Normal ifadeler konumu yönergesinde ngx_http_rewrite_module için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="80cc6-190">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="80cc6-191">Http_ssl_module HTTPS protokolü desteği ekler.</span><span class="sxs-lookup"><span data-stu-id="80cc6-191">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="80cc6-192">Bir web uygulaması güvenlik duvarı gibi kullanmayı *ModSecurity* uygulama sağlamlaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="80cc6-192">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="80cc6-193">SSL yapılandırma</span><span class="sxs-lookup"><span data-stu-id="80cc6-193">Configure SSL</span></span>

* <span data-ttu-id="80cc6-194">Bağlantı noktasında HTTPS trafiğini dinlemek üzere yapılandırmak `443` bir güvenilen sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek.</span><span class="sxs-lookup"><span data-stu-id="80cc6-194">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="80cc6-195">Aşağıda gösterilen uygulamaları kullanan güvenliğini sağlamlaştırmak */etc/nginx/nginx.conf* dosya.</span><span class="sxs-lookup"><span data-stu-id="80cc6-195">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="80cc6-196">Örnek daha güçlü şifreleme seçme ve HTTPS için HTTP üzerinden tüm trafiği yönlendirerek verilebilir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-196">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="80cc6-197">Ekleme bir `HTTP Strict-Transport-Security` (HSTS) üstbilgisi sağlar, HTTPS üzerinden yalnızca istemci tarafından yapılan tüm sonraki istekleri.</span><span class="sxs-lookup"><span data-stu-id="80cc6-197">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="80cc6-198">Strict Aktarım güvenlik üstbilgisi eklemeyin veya uygun bir seçtiğiniz `max-age` SSL gelecekte devre dışıysa.</span><span class="sxs-lookup"><span data-stu-id="80cc6-198">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="80cc6-199">Ekleme */etc/nginx/proxy.conf* yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="80cc6-199">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="80cc6-200">Düzen */etc/nginx/nginx.conf* yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="80cc6-200">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="80cc6-201">Örnek içeren `http` ve `server` bölümler bir yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="80cc6-201">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="80cc6-202">Clickjacking öğesini gelen güvenli nginx</span><span class="sxs-lookup"><span data-stu-id="80cc6-202">Secure nginx from clickjacking</span></span>
<span data-ttu-id="80cc6-203">Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="80cc6-203">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="80cc6-204">Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları.</span><span class="sxs-lookup"><span data-stu-id="80cc6-204">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="80cc6-205">Kullanım X-FRAME-site güvenli hale getirmek için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="80cc6-205">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="80cc6-206">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="80cc6-206">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="80cc6-207">Satırı ekleyin `add_header X-Frame-Options "SAMEORIGIN";` ve dosyayı kaydedin, sonra nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="80cc6-207">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="80cc6-208">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="80cc6-208">MIME-type sniffing</span></span>

<span data-ttu-id="80cc6-209">Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması çoğu tarayıcılarından yanıt içerik türü, yöntemi bir kenara bırakarak engeller.</span><span class="sxs-lookup"><span data-stu-id="80cc6-209">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="80cc6-210">İle `nosniff` seçeneği, sunucunun içeriktir "text/html" diyorsa, tarayıcı, "text/html" işler.</span><span class="sxs-lookup"><span data-stu-id="80cc6-210">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="80cc6-211">Düzen *nginx.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="80cc6-211">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="80cc6-212">Satırı ekleyin `add_header X-Content-Type-Options "nosniff";` ve dosyayı kaydedin, sonra nginx yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="80cc6-212">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart nginx.</span></span>
