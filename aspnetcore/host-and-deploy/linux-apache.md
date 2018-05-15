---
title: ASP.NET Core Apache ile Linux ana bilgisayar
description: Apache CentOS üzerinde ters proxy sunucu olarak Kestrel üzerinde çalışan bir ASP.NET Core web uygulaması için HTTP trafiği yönlendirmek için nasıl ayarlanacağını öğrenin.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="af12d-103">ASP.NET Core Apache ile Linux ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="af12d-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="af12d-104">Tarafından [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="af12d-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="af12d-105">Bu kılavuz kullanılarak nasıl ayarlanacağını öğrenin [Apache](https://httpd.apache.org/) ters proxy sunucusu olarak [CentOS 7](https://www.centos.org/) üzerinde çalışan bir ASP.NET Core web uygulaması için HTTP trafiği yönlendirmek için [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="af12d-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="af12d-106">[Mod_proxy uzantısı](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) ve ilgili modüller sunucunun ters proxy oluşturun.</span><span class="sxs-lookup"><span data-stu-id="af12d-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af12d-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="af12d-107">Prerequisites</span></span>

1. <span data-ttu-id="af12d-108">Sudo ayrıcalığa sahip standart kullanıcı hesabı ile CentOS 7'yi çalıştıran sunucu</span><span class="sxs-lookup"><span data-stu-id="af12d-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="af12d-109">ASP.NET Core uygulama</span><span class="sxs-lookup"><span data-stu-id="af12d-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="af12d-110">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="af12d-110">Publish the app</span></span>

<span data-ttu-id="af12d-111">Bir uygulama olarak yayımlama bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) CentOS 7 çalışma zamanı için sürüm yapılandırmasında (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="af12d-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="af12d-112">İçeriğini kopyalayın *bin/Release/netcoreapp2.0/centos.7-x64/publish* SCP, FTP veya başka bir dosya aktarım yöntemi kullanarak sunucu klasörüne.</span><span class="sxs-lookup"><span data-stu-id="af12d-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="af12d-113">Bir üretim dağıtım senaryosunda sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="af12d-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="af12d-114">Bir proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="af12d-114">Configure a proxy server</span></span>

<span data-ttu-id="af12d-115">Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="af12d-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="af12d-116">Ters proxy HTTP isteği sonlandırır ve ASP.NET uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="af12d-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="af12d-117">Bir proxy sunucusu, istemci isteklerini istekleri kendisi yerine getirmesini yerine başka bir sunucuya iletir biridir.</span><span class="sxs-lookup"><span data-stu-id="af12d-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="af12d-118">Ters proxy genellikle rastgele istemcileri adına sabit bir hedef iletir.</span><span class="sxs-lookup"><span data-stu-id="af12d-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="af12d-119">Bu kılavuzda, Apache Kestrel ASP.NET Core uygulama hizmet ettiğini aynı sunucu üzerinde çalışan ters proxy yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="af12d-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="af12d-120">İstekleri tarafından ters proxy iletilir çünkü iletilen üstbilgileri Ara kullanmak [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="af12d-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="af12d-121">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="af12d-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="af12d-122">Kimlik doğrulaması ara yazılımı herhangi bir türde kullanırken, iletilen üstbilgileri Ara ilk çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="af12d-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="af12d-123">Bu sıralama, kimlik doğrulaması ara yazılımı üstbilgi değerlerini kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="af12d-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="af12d-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="af12d-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="af12d-125">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulama düzeni ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="af12d-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="af12d-126">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="af12d-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="af12d-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="af12d-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="af12d-128">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer kimlik doğrulama şeması Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="af12d-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="af12d-129">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="af12d-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="af12d-130">Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan üstbilgiler `None`.</span><span class="sxs-lookup"><span data-stu-id="af12d-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="af12d-131">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="af12d-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="af12d-132">Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="af12d-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="af12d-133">Apache yükleyin</span><span class="sxs-lookup"><span data-stu-id="af12d-133">Install Apache</span></span>

<span data-ttu-id="af12d-134">CentOS paketleri en son kararlı sürümlerine güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="af12d-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="af12d-135">Apache web sunucusu üzerinde CentOS tek bir yükleme `yum` komutu:</span><span class="sxs-lookup"><span data-stu-id="af12d-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="af12d-136">Komutu çalıştırdıktan sonra çıktı örneği:</span><span class="sxs-lookup"><span data-stu-id="af12d-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="af12d-137">Bu örnekte, CentOS 7 sürümü 64-bit olduğundan çıktı httpd.86_64 yansıtır.</span><span class="sxs-lookup"><span data-stu-id="af12d-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="af12d-138">Apache yüklendiği doğrulamak için çalıştırın `whereis httpd` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="af12d-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="af12d-139">Apache için ters proxy ayarlarını yapılandır</span><span class="sxs-lookup"><span data-stu-id="af12d-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="af12d-140">İçinde Apache için yapılandırma dosyalarının bulunduğu `/etc/httpd/conf.d/` dizin.</span><span class="sxs-lookup"><span data-stu-id="af12d-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="af12d-141">Herhangi dosya ile *.conf* uzantısı modül yapılandırma dosyalarında yanı sıra alfabetik sırada işlenir `/etc/httpd/conf.modules.d/`, herhangi bir yapılandırma içeren modüllerini yüklemek gerekli dosyaları.</span><span class="sxs-lookup"><span data-stu-id="af12d-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="af12d-142">Adlı bir yapılandırma dosyası oluşturma *hellomvc.conf*, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="af12d-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="af12d-143">`VirtualHost` Blok, bir sunucu üzerindeki bir veya daha fazla dosyalarda birden çok kez görüntülenebilir.</span><span class="sxs-lookup"><span data-stu-id="af12d-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="af12d-144">Önceki yapılandırma dosyasında bağlantı noktası 80 üzerinde ortak trafiğin Apache kabul eder.</span><span class="sxs-lookup"><span data-stu-id="af12d-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="af12d-145">Etki alanı `www.example.com` sunulmasını ve `*.example.com` diğer ad aynı Web sitesine giderir.</span><span class="sxs-lookup"><span data-stu-id="af12d-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="af12d-146">Bkz: [sanal ana bilgisayar adı tabanlı destek](https://httpd.apache.org/docs/current/vhosts/name-based.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="af12d-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="af12d-147">İstekleri kökündeki 127.0.0.1 server örneğinin 5000 numaralı bağlantı noktasına taşınır.</span><span class="sxs-lookup"><span data-stu-id="af12d-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="af12d-148">Çift yönlü iletişimi için `ProxyPass` ve `ProxyPassReverse` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="af12d-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="af12d-149">Uygun belirtmek için hata [ServerName yönergesi](https://httpd.apache.org/docs/current/mod/core.html#servername) içinde **VirtualHost** blok güvenlik açıkları, uygulamanızın kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="af12d-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="af12d-150">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanı denetlemek, bu güvenlik riski değil (tersine `*.com`, açık olduğu).</span><span class="sxs-lookup"><span data-stu-id="af12d-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="af12d-151">Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="af12d-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="af12d-152">Günlüğe kaydetme, başına yapılandırılabilir `VirtualHost` kullanarak `ErrorLog` ve `CustomLog` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="af12d-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="af12d-153">`ErrorLog` Burada sunucusu günlüklerini hataları, konumu ve `CustomLog` filename ve günlük dosyası biçimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="af12d-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="af12d-154">Bu durumda, istek bilgilerini günlüğe nerede budur.</span><span class="sxs-lookup"><span data-stu-id="af12d-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="af12d-155">Her istek için bir satır vardır.</span><span class="sxs-lookup"><span data-stu-id="af12d-155">There's one line for each request.</span></span>

<span data-ttu-id="af12d-156">Dosyayı kaydedin ve yapılandırmayı test etme.</span><span class="sxs-lookup"><span data-stu-id="af12d-156">Save the file and test the configuration.</span></span> <span data-ttu-id="af12d-157">Her şeyi geçerse, yanıt olmalıdır `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="af12d-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="af12d-158">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="af12d-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="af12d-159">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="af12d-159">Monitoring the app</span></span>

<span data-ttu-id="af12d-160">Apache olan şimdi yapılan isteklerini iletmek için kurulumu `http://localhost:80` Kestrel çalışan ASP.NET Core App `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="af12d-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="af12d-161">Ancak, Apache Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="af12d-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="af12d-162">Kullanım *systemd* ve Başlat ve temel web uygulaması izlemek için bir hizmet dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="af12d-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="af12d-163">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="af12d-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="af12d-164">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="af12d-164">Create the service file</span></span>

<span data-ttu-id="af12d-165">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="af12d-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="af12d-166">Uygulama için bir örnek hizmet dosyası:</span><span class="sxs-lookup"><span data-stu-id="af12d-166">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

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
> <span data-ttu-id="af12d-167">**Kullanıcı** &mdash; , kullanıcı *apache* kullanılmaz yapılandırmanın, kullanıcı ilk oluşturulmalı ve doğru sahipliği dosyaları için verilen.</span><span class="sxs-lookup"><span data-stu-id="af12d-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="af12d-168">Ortam değişkenleri okumak yapılandırma sağlayıcısı için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçış uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="af12d-168">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="af12d-169">Yapılandırma dosyasında kullanmak için düzgün bir şekilde Atlanan değeri oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="af12d-169">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="af12d-170">Dosyayı kaydedin ve hizmeti etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="af12d-170">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="af12d-171">Hizmeti başlatın ve çalıştığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="af12d-171">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="af12d-172">Ters proxy yapılandırılmış ve üzerinden yönetilen Kestrel *systemd*, web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="af12d-172">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="af12d-173">Yanıt Üstbilgileri incelemek **Server** üstbilgi gösterir ASP.NET Core uygulama tarafından Kestrel sunulur:</span><span class="sxs-lookup"><span data-stu-id="af12d-173">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="af12d-174">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="af12d-174">Viewing logs</span></span>

<span data-ttu-id="af12d-175">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir *systemd*, olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="af12d-175">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="af12d-176">Ancak, bu günlük tüm işlemler tarafından yönetilen ve Hizmetleri için girişleri içerir *systemd*.</span><span class="sxs-lookup"><span data-stu-id="af12d-176">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="af12d-177">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="af12d-177">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="af12d-178">Zaman filtresi uygulamak için komutuyla zaman seçeneklerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="af12d-178">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="af12d-179">Örneğin, `--since today` geçerli gün için filtre uygulamak için veya `--until 1 hour ago` önceki saatlik girişlerini görmek için.</span><span class="sxs-lookup"><span data-stu-id="af12d-179">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="af12d-180">Daha fazla bilgi için bkz: [journalctl adam sayfa](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="af12d-180">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="af12d-181">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="af12d-181">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="af12d-182">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="af12d-182">Configure firewall</span></span>

<span data-ttu-id="af12d-183">*Firewalld* ağ bölgeleri için destek ile Güvenlik Duvarı'nı yönetmek için bir dinamik arka plan programı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="af12d-183">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="af12d-184">Bağlantı noktaları ve paket filtreleme hala iptables tarafından yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="af12d-184">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="af12d-185">*Firewalld* varsayılan olarak yüklü olması.</span><span class="sxs-lookup"><span data-stu-id="af12d-185">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="af12d-186">`yum` paketi yüklemek ya da yüklü doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="af12d-186">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="af12d-187">Kullanım `firewalld` yalnızca uygulama için gerekli bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="af12d-187">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="af12d-188">Bu durumda, bağlantı noktası 80 ve 443 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="af12d-188">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="af12d-189">Aşağıdaki komutlar, 80 ve 443'ü açmak için bağlantı noktalarını kalıcı olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="af12d-189">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="af12d-190">Güvenlik Duvarı ayarlarını yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="af12d-190">Reload the firewall settings.</span></span> <span data-ttu-id="af12d-191">Kullanılabilir hizmetleri ve bağlantı noktalarını varsayılan bölgede denetleyin.</span><span class="sxs-lookup"><span data-stu-id="af12d-191">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="af12d-192">Seçenekleri kullanılabilir inceleyerek `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="af12d-192">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="af12d-193">SSL yapılandırması</span><span class="sxs-lookup"><span data-stu-id="af12d-193">SSL configuration</span></span>

<span data-ttu-id="af12d-194">SSL, Apache yapılandırmak için *mod_ssl* modülü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="af12d-194">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="af12d-195">Zaman *httpd* modülü yüklendi, *mod_ssl* modülü de yüklendi.</span><span class="sxs-lookup"><span data-stu-id="af12d-195">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="af12d-196">Yüklü değildi kullanırsanız `yum` yapılandırmasına eklemek için.</span><span class="sxs-lookup"><span data-stu-id="af12d-196">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="af12d-197">SSL zorlamak için yükleme `mod_rewrite` modülü URL yeniden yazma işlemi etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="af12d-197">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="af12d-198">Değiştirme *hellomvc.conf* dosya URL yeniden yazma işlemi etkinleştirmek ve bağlantı noktası 443 üzerinden iletişimi güvenli hale getirmek için:</span><span class="sxs-lookup"><span data-stu-id="af12d-198">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
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
> <span data-ttu-id="af12d-199">Bu örnek, yerel olarak oluşturulan bir sertifika kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="af12d-199">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="af12d-200">**SSLCertificateFile** birincil sertifika dosyası için etki alanı adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="af12d-200">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="af12d-201">**SSLCertificateKeyFile** CSR oluşturulduğunda anahtar dosyası oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="af12d-201">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="af12d-202">**SSLCertificateChainFile** ara sertifika dosyası (varsa) olmalıdır sertifika yetkilisi tarafından sağlandı.</span><span class="sxs-lookup"><span data-stu-id="af12d-202">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="af12d-203">Dosyayı kaydedin ve test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="af12d-203">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="af12d-204">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="af12d-204">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="af12d-205">Ek Apache öneriler</span><span class="sxs-lookup"><span data-stu-id="af12d-205">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="af12d-206">Ek üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="af12d-206">Additional headers</span></span>

<span data-ttu-id="af12d-207">Kötü amaçlı saldırılara karşı güvenli hale getirmek için ya da değiştirildiğinde veya eklendiğinde birkaç üstbilgileri vardır.</span><span class="sxs-lookup"><span data-stu-id="af12d-207">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="af12d-208">Emin `mod_headers` modülü yüklenir:</span><span class="sxs-lookup"><span data-stu-id="af12d-208">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="af12d-209">Clickjacking öğesini saldırılarına karşı güvenli Apache</span><span class="sxs-lookup"><span data-stu-id="af12d-209">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="af12d-210">[Clickjacking öğesini](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), olarak da bilinen bir *UI redress saldırı*, bir Web sitesine ziyaretçiyi yere sağladı şu anda ziyaret ettiğiniz daha bir bağlantı veya başka bir sayfaya düğmesine tıklamak amaçlı bir saldırı aracıdır.</span><span class="sxs-lookup"><span data-stu-id="af12d-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="af12d-211">Kullanım `X-FRAME-OPTIONS` sitesi güvenliğini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="af12d-211">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="af12d-212">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="af12d-212">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="af12d-213">Satırı ekleyin `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="af12d-213">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="af12d-214">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="af12d-214">Save the file.</span></span> <span data-ttu-id="af12d-215">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="af12d-215">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="af12d-216">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="af12d-216">MIME-type sniffing</span></span>

<span data-ttu-id="af12d-217">`X-Content-Type-Options` Üstbilgi engeller Internet Explorer'dan *MIME algılaması* (bir dosyanın belirleme `Content-Type` dosyanın içerikten).</span><span class="sxs-lookup"><span data-stu-id="af12d-217">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="af12d-218">Sunucu ayarlarsa `Content-Type` başlığına `text/html` ile `nosniff` seçenek kümesi, Internet Explorer işler içeriği olarak `text/html` dosyanın içeriği ne olursa olsun.</span><span class="sxs-lookup"><span data-stu-id="af12d-218">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="af12d-219">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="af12d-219">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="af12d-220">Satırı ekleyin `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="af12d-220">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="af12d-221">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="af12d-221">Save the file.</span></span> <span data-ttu-id="af12d-222">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="af12d-222">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="af12d-223">YükDengeleme</span><span class="sxs-lookup"><span data-stu-id="af12d-223">Load Balancing</span></span> 

<span data-ttu-id="af12d-224">Bu örnekte, Kurulum ve Apache CentOS 7 ve Kestrel aynı örneği makinede yapılandırmak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="af12d-224">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="af12d-225">Tek bir hata noktası olmaması için; kullanarak *mod_proxy_balancer* ve değiştirme **VirtualHost** web uygulamaları Apache proxy sunucunun arkasında birden çok örneğini yönetmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="af12d-225">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="af12d-226">Yapılandırma dosyasında ek bir örneği aşağıda gösterilen `hellomvc` 5001 bağlantı noktası üzerinde çalıştırmak için Kurulum uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="af12d-226">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="af12d-227">*Proxy* bölüm ayarlanmış iki üyeleriyle dengeleyici yapılandırmasına sahip Yük Dengelemesi *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="af12d-227">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
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

### <a name="rate-limits"></a><span data-ttu-id="af12d-228">Oran sınırları</span><span class="sxs-lookup"><span data-stu-id="af12d-228">Rate Limits</span></span>
<span data-ttu-id="af12d-229">Kullanarak *mod_ratelimit*, içinde bulunan *httpd* modülü, bant genişliği istemcilerin olabilir sınırlı:</span><span class="sxs-lookup"><span data-stu-id="af12d-229">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="af12d-230">Örnek dosyası kök konumu altında 600 KB/sn olarak bant genişliği sınırları:</span><span class="sxs-lookup"><span data-stu-id="af12d-230">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
