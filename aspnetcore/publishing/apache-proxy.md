---
title: ASP.NET Core Apache ile Linux ana bilgisayar
description: "Apache CentOS üzerinde ters proxy sunucu olarak Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması için HTTP trafiği yönlendirmek için nasıl ayarlanacağını öğrenin."
keywords: "ASP.NET Core, Apache, CentOS, Ters Proxy, Linux, mod_proxy, httpd, barındırma"
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="dd7a6-104">ASP.NET Core Apache ile Linux için bir barındırma ortamını ayarlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="dd7a6-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="dd7a6-105">Tarafından [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="dd7a6-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="dd7a6-106">Apache çok yaygın bir HTTP sunucusudur ve HTTP trafiği için nginx benzer yönlendirmek için bir proxy olarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="dd7a6-107">Bu kılavuzda, biz CentOS 7 üzerinde Apache ayarlama ve gelen bağlantıları Hoş Geldiniz ve Kestrel üzerinde çalışan ASP.NET Core uygulama yönlendirmek için ters proxy kullanmak üzere öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="dd7a6-108">Bu amaçla kullanacağız *mod_proxy* uzantısı ve ilgili diğer Apache modüller.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd7a6-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="dd7a6-109">Prerequisites</span></span>

1. <span data-ttu-id="dd7a6-110">Sudo ayrıcalığa sahip standart kullanıcı hesabı ile CentOS 7 çalıştıran bir sunucu.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="dd7a6-111">Mevcut bir ASP.NET Core uygulama.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="dd7a6-112">Uygulamanızı yayımlama</span><span class="sxs-lookup"><span data-stu-id="dd7a6-112">Publish your application</span></span>

<span data-ttu-id="dd7a6-113">Çalıştırma `dotnet publish -c Release` sunucunuzda çalışan kendi içinde bulunan bir dizin uygulamanıza paketlemek için geliştirme ortamınızı gelen.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="dd7a6-114">Yayımlanan uygulama sonra SCP, FTP veya başka bir dosya aktarım yöntemi kullanarak sunucusuna kopyalanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="dd7a6-115">Bir üretim dağıtım senaryosunda sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="dd7a6-116">Bir proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="dd7a6-116">Configure a proxy server</span></span>

<span data-ttu-id="dd7a6-117">Ters proxy dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="dd7a6-118">Ters proxy HTTP isteği sonlandırır ve ASP.NET uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="dd7a6-119">Bir proxy sunucusu, istemci isteklerini yerine bunları kendisi yerine getiren başka bir sunucuya iletir biridir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="dd7a6-120">Ters proxy genellikle rastgele istemcileri adına sabit bir hedef iletir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="dd7a6-121">Bu kılavuzda, Apache Kestrel ASP.NET Core uygulama hizmet ettiğini aynı sunucu üzerinde çalışan ters proxy olarak yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="dd7a6-122">Her bir uygulamanın parçası ayrı fiziksel makineler, Docker kapsayıcıları veya yapılandırmaları mimari gereksinimleri veya sınırlamaları bağlı olarak birlikte bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="dd7a6-123">Apache yükleyin</span><span class="sxs-lookup"><span data-stu-id="dd7a6-123">Install Apache</span></span>

<span data-ttu-id="dd7a6-124">Apache web sunucusu üzerinde CentOS yüklemektir ilk ancak tek bir komut şimdi paketlerimize güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="dd7a6-125">Bu, tüm yüklü paketleri kendi en son sürümüne güncelleştirilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="dd7a6-126">Apache kullanarak yükleme`yum`</span><span class="sxs-lookup"><span data-stu-id="dd7a6-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="dd7a6-127">Çıktı aşağıdakine benzer bir şey yansıtmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-127">The output should reflect something similar to the following.</span></span>

```bash
    Downloading packages:
    httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01     
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
    Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
    Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

    Installed:
    httpd.x86_64 0:2.4.6-40.el7.centos.4                                                                           

    Complete!
```

> [!NOTE]
> <span data-ttu-id="dd7a6-128">Bu örnekte, CentOS 7 sürümü 64-bit olduğundan çıktı httpd.86_64 yansıtır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="dd7a6-129">Çıktı sunucunuz için farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-129">The output may be different for your server.</span></span> <span data-ttu-id="dd7a6-130">Apache yüklendiği doğrulamak için çalıştırın `whereis httpd` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="dd7a6-131">Apache için ters proxy ayarlarını yapılandır</span><span class="sxs-lookup"><span data-stu-id="dd7a6-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="dd7a6-132">İçinde Apache için yapılandırma dosyalarının bulunduğu `/etc/httpd/conf.d/` dizin.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="dd7a6-133">Herhangi dosya ile **.conf** uzantısı, modül yapılandırma dosyalarında yanı sıra alfabetik sırada işlenir `/etc/httpd/conf.modules.d/`, herhangi bir yapılandırma içeren modüllerini yüklemek gerekli dosyaları.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="dd7a6-134">Bu arayacağız Bu örnek için uygulamanız için bir yapılandırma dosyası oluşturma`hellomvc.conf`</span><span class="sxs-lookup"><span data-stu-id="dd7a6-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="dd7a6-135">*VirtualHost* hangisinin olabilir birden çok düğüm, bir dosyada ya da çok sayıda dosya sunucusundaki 80 numaralı bağlantı noktasını kullanarak herhangi bir IP adresi dinleyecek şekilde ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="dd7a6-136">Sonraki iki satır 5000 makine 127.0.0.1 noktasına ve geriye doğru kökündeki alınan tüm istekleri geçirmek için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="dd7a6-137">Çift yönlü iletişim, her iki ayarın olmasını orada için *ProxyPass* ve *ProxyPassReverse* gereklidir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="dd7a6-138">Günlük VirtualHost yapılandırılabilir kullanarak *hata günlüğüne* ve *CustomLog* yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="dd7a6-139">*Hata günlüğü* sunucusu günlük nerede hataları konumu ve *CustomLog* filename ve günlük dosyası biçimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="dd7a6-140">Örneğimizde isteği bilgilerini günlüğe nerede budur.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="dd7a6-141">Her istek için bir satır olacak.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-141">There will be one line for each request.</span></span>

<span data-ttu-id="dd7a6-142">Dosyayı kaydedin ve yapılandırmayı test etme.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="dd7a6-143">Her şeyi geçerse, yanıt olmalıdır `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="dd7a6-144">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="dd7a6-145">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="dd7a6-145">Monitoring our application</span></span>

<span data-ttu-id="dd7a6-146">Apache olan şimdi yapılan isteklerini iletmek için kurulumu `http://localhost:80` Kestrel çalışan ASP.NET Core uygulama açın `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="dd7a6-147">Ancak, Apache Kestrel işlemini yönetmek üzere ayarlanmış değil.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="dd7a6-148">Kullanacağız *systemd* ve Başlat ve temel web uygulaması izlemek için bir hizmet dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="dd7a6-149">*systemd* , başlatma, durdurma ve işlemlerini yönetme pek çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="dd7a6-150">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="dd7a6-150">Create the service file</span></span>

<span data-ttu-id="dd7a6-151">Hizmet tanımı dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="dd7a6-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="dd7a6-152">Uygulamamız bir örnek hizmet dosyası.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

    [Service]
    WorkingDirectory=/var/aspnetcore/hellomvc
    ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
    Restart=always
    # Restart service after 10 seconds if dotnet service crashes
    RestartSec=10
    SyslogIdentifier=dotnet-example
    User=apache
    Environment=ASPNETCORE_ENVIRONMENT=Production 

    [Install]
    WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="dd7a6-153">**Kullanıcı** --If kullanıcı *apache* kullanılmaz yapılandırmanıza göre burada tanımlanan kullanıcı ilk oluşturulmalı ve dosyaları için doğru sahipliği verilen</span><span class="sxs-lookup"><span data-stu-id="dd7a6-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="dd7a6-154">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="dd7a6-155">Hizmeti başlatın ve çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-155">Start the service and verify that it is running.</span></span>

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="dd7a6-156">Web uygulaması yapılandırılmış ters proxy ve systemd yönetilen Kestrel ile tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="dd7a6-157">Yanıt Üstbilgileri incelemek **Server** Kestrel tarafından sunulmasını ASP.NET Core uygulama görüntülenmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="dd7a6-158">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="dd7a6-158">Viewing logs</span></span>

<span data-ttu-id="dd7a6-159">Kestrel kullanarak web uygulamasına systemd kullanılarak yönetilir olduğundan, tüm olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="dd7a6-160">Ancak, bu günlük tüm hizmetleri ve işlemleri systemd tarafından yönetilen tüm girişleri içerir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="dd7a6-161">Görüntülemek için `kestrel-hellomvc.service` belirli öğeleri, aşağıdaki komutu kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="dd7a6-162">Daha fazla filtrelemek için zaman seçenekleri gibi `--since today`, `--until 1 hour ago` veya bunların bir birleşimi döndürülen girişleri azaltmak.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="dd7a6-163">Uygulamamız güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="dd7a6-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="dd7a6-164">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7a6-164">Configure firewall</span></span>

<span data-ttu-id="dd7a6-165">*Firewalld* bağlantı noktalarını ve paket filtreleme yönetmek için iptables kullanmaya devam edebilirsiniz ancak ağ bölgeleri için destek ile Güvenlik Duvarı'nı yönetmek için bir dinamik arka plan programı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="dd7a6-166">Firewalld, varsayılan olarak yüklü olması `yum` paketi yükleyin veya doğrulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="dd7a6-167">Kullanarak `firewalld` yalnızca uygulama için gerekli bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="dd7a6-168">Bu durumda, bağlantı noktası 80 ve 443 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="dd7a6-169">Aşağıdaki komutları kalıcı olarak ayarlar bu açın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="dd7a6-170">Güvenlik Duvarı ayarlarını yeniden yükleyin ve kullanılabilir hizmetleri ve bağlantı noktalarını varsayılan bölgede denetleyin.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="dd7a6-171">İnceleyerek seçenekleri kullanılabilir`firewall-cmd -h`</span><span class="sxs-lookup"><span data-stu-id="dd7a6-171">Options are available by inspecting `firewall-cmd -h`</span></span>

```bash 
    sudo firewall-cmd --reload
    sudo firewall-cmd --list-all
```

```bash
    public (default, active)
    interfaces: eth0
    sources: 
    services: dhcpv6-client
    ports: 443/tcp 80/tcp
    masquerade: no
    forward-ports: 
    icmp-blocks: 
    rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="dd7a6-172">SSL yapılandırması</span><span class="sxs-lookup"><span data-stu-id="dd7a6-172">SSL configuration</span></span>

<span data-ttu-id="dd7a6-173">SSL için Apache yapılandırmak için mod_ssl modülü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="dd7a6-174">Biz yüklendiğinde bu başlangıçta yüklendi `httpd` modülü.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="dd7a6-175">Eksik veya yüklü değil, yum yapılandırmanıza eklemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="dd7a6-176">SSL zorlamak için yükleyin`mod_rewrite`</span><span class="sxs-lookup"><span data-stu-id="dd7a6-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="dd7a6-177">`hellomvc.conf` Bu örnek yeniden yanı sıra yeni ekleme etkinleştirmek için değiştirilmesi gereken için oluşturulmuş olan dosyasını **VirtualHost** HTTPS için bölüm.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

```text
    <VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{HTTPS} !=on
        RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
    </VirtualHost>

    <VirtualHost *:443>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
        SSLEngine on
        SSLProtocol all -SSLv2
        SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
        SSLCertificateFile /etc/pki/tls/certs/localhost.crt
        SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    </VirtualHost>    
```

> [!NOTE]
> <span data-ttu-id="dd7a6-178">Bu örnek, yerel olarak oluşturulan bir sertifika kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="dd7a6-179">**SSLCertificateFile** birincil sertifikası dosyanız için etki alanı adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="dd7a6-180">**SSLCertificateKeyFile** CSR oluşturduğunuzda oluşturulan anahtar dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="dd7a6-181">**SSLCertificateChainFile** ara sertifika dosyası (varsa) olmalıdır, sertifika yetkilisi tarafından sağlandı</span><span class="sxs-lookup"><span data-stu-id="dd7a6-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="dd7a6-182">Dosyayı kaydedin ve yapılandırmayı test etme.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="dd7a6-183">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="dd7a6-184">Ek Apache öneriler</span><span class="sxs-lookup"><span data-stu-id="dd7a6-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="dd7a6-185">Ek üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="dd7a6-185">Additional Headers</span></span> 
<span data-ttu-id="dd7a6-186">Karşı güvenliğini sağlamak için kötü amaçlı saldırıları da değiştirildiğinde veya eklendiğinde birkaç üstbilgileri var.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="dd7a6-187">Emin `mod_headers` Modülü yüklüdür.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="dd7a6-188">Clickjacking öğesini gelen güvenli Apache</span><span class="sxs-lookup"><span data-stu-id="dd7a6-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="dd7a6-189">Clickjacking öğesini etkilenen kullanıcının tıklama toplamak için kötü amaçlı bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="dd7a6-190">Clickjacking öğesini, etkilenen bir sitede tıklamak (ziyaretçi) kaybeden püf noktaları.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="dd7a6-191">Kullanım X-FRAME-sitenizin güvenliğini sağlamak için OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="dd7a6-192">Httpd.conf dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="dd7a6-193">Satırı ekleyin `Header append X-FRAME-OPTIONS "SAMEORIGIN"` ve dosyayı kaydedin, sonra da Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="dd7a6-194">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="dd7a6-194">MIME-type sniffing</span></span>

<span data-ttu-id="dd7a6-195">Üstbilgi yanıt içerik türü geçersiz tarayıcıyı yönlendirir gibi bu başlığı MIME algılaması Internet Explorer'dan bildirilen content-type çıktığınızda yanıt engeller.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="dd7a6-196">Sunucu metin/html, içeriktir diyorsa nosniff seçeneğiyle tarayıcı, text/html olarak kabul eder.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="dd7a6-197">Httpd.conf dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="dd7a6-198">Satırı ekleyin `Header set X-Content-Type-Options "nosniff"` ve dosyayı kaydedin, sonra da Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="dd7a6-199">YükDengeleme</span><span class="sxs-lookup"><span data-stu-id="dd7a6-199">Load Balancing</span></span> 

<span data-ttu-id="dd7a6-200">Bu örnekte, Kurulum ve Apache CentOS 7 ve Kestrel aynı örneği makinede yapılandırmak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="dd7a6-201">Ancak, tek bir hata; noktası olmaması için kullanarak *mod_proxy_balancer* ve VirtualHost değiştirmeye izin web uygulamaları Apache proxy sunucunun arkasında birden çok örneğini yönetmek için.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="dd7a6-202">Yapılandırma dosyası, ek bir örneği `hellomvc` uygulama 5001 bağlantı noktası üzerinde çalıştırmak için Kurulum açıldı ve *Proxy* bölüm ayarlanmış iki üyeleriyle dengeleyici yapılandırmasına sahip yükünü dengelemek için *byrequests* .</span><span class="sxs-lookup"><span data-stu-id="dd7a6-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```text
    <VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{HTTPS} !=on
        RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
    </VirtualHost>

    <VirtualHost *:443>
            ProxyPass / balancer://mycluster/ 

            ProxyPassReverse / http://127.0.0.1:5000/
            ProxyPassReverse / http://127.0.0.1:5001/

            <Proxy balancer://mycluster>
                BalancerMember http://127.0.0.1:5000
                BalancerMember http://127.0.0.1:5001 
                ProxySet lbmethod=byrequests
            </Proxy>

            <Location />
                SetHandler balancer
            </Location>
            ErrorLog /var/log/httpd/hellomvc-error.log
            CustomLog /var/log/httpd/hellomvc-access.log common
            SSLEngine on
            SSLProtocol all -SSLv2
            SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
            SSLCertificateFile /etc/pki/tls/certs/localhost.crt
            SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    </VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="dd7a6-203">Oran sınırları</span><span class="sxs-lookup"><span data-stu-id="dd7a6-203">Rate Limits</span></span>
<span data-ttu-id="dd7a6-204">Kullanarak `mod_ratelimit`, içinde bulunan `htttpd` istemciler, bant genişliği miktarını sınırla modülü.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="dd7a6-205">Örnek dosyası kök konumu altında 600 KB/sn olarak bant genişliği sınırlar.</span><span class="sxs-lookup"><span data-stu-id="dd7a6-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
