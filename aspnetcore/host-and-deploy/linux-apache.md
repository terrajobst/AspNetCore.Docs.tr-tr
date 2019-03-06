---
title: ASP.NET Core Apache ile Linux'ta barındırma
description: Apache CentOS, ters Ara sunucu olarak Kestrel üzerinde çalışan ASP.NET Core web uygulaması için HTTP trafiğini yönlendirmek için nasıl kuracağınızı öğrenin.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 02/27/2019
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 69026997b2c269a4fb56ed2a79fa42ae218368e1
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345944"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="18e60-103">ASP.NET Core Apache ile Linux'ta barındırma</span><span class="sxs-lookup"><span data-stu-id="18e60-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="18e60-104">Tarafından [Shayne boyer'ın](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="18e60-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="18e60-105">Bu kılavuz kullanılarak nasıl ayarlanacağını öğrenin [Apache](https://httpd.apache.org/) ters Ara sunucu olarak [CentOS 7](https://www.centos.org/) üzerinde çalışan ASP.NET Core web uygulaması için HTTP trafiğini yönlendirmek için [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="18e60-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="18e60-106">[Mod_proxy uzantısı](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) ve ilgili modüller ters proxy sunucunun oluşturun.</span><span class="sxs-lookup"><span data-stu-id="18e60-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18e60-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="18e60-107">Prerequisites</span></span>

* <span data-ttu-id="18e60-108">Sudo ayrıcalıklarıyla standart kullanıcı hesabı ile CentOS 7 çalıştıran sunucu.</span><span class="sxs-lookup"><span data-stu-id="18e60-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="18e60-109">.NET Core çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="18e60-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="18e60-110">Ziyaret [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="18e60-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="18e60-111">En son Önizleme çalışma zamanı altında listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="18e60-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="18e60-112">Seçin ve CentOS/Oracle için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="18e60-112">Select and follow the instructions for CentOS/Oracle.</span></span>
* <span data-ttu-id="18e60-113">Mevcut bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="18e60-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="18e60-114">Yayımlama ve uygulamanın üzerine kopyalayın</span><span class="sxs-lookup"><span data-stu-id="18e60-114">Publish and copy over the app</span></span>

<span data-ttu-id="18e60-115">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="18e60-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="18e60-116">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulamayı paketlemek için geliştirme ortamından (örneğin, *bin/yayın/&lt;target_framework_moniker&gt;/ publish*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="18e60-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="18e60-117">Uygulama ayrıca olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) .NET Core çalışma zamanı sunucuda değil sağlamak isterseniz.</span><span class="sxs-lookup"><span data-stu-id="18e60-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="18e60-118">ASP.NET Core uygulaması (örneğin, SCP, SFTP) kuruluşun iş akışınıza tümleştirir bir aracını kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="18e60-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="18e60-119">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *www/var/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="18e60-119">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="18e60-120">Üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="18e60-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="18e60-121">Bir proxy sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="18e60-121">Configure a proxy server</span></span>

<span data-ttu-id="18e60-122">Ters proxy hizmet dinamik web uygulamaları için ortak bir kurulum var.</span><span class="sxs-lookup"><span data-stu-id="18e60-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="18e60-123">Ters proxy, HTTP isteği sonlandırır ve ASP.NET uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="18e60-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="18e60-124">Bir proxy sunucusu, istemci istekleri istekleri kendisi yerine getirmesini yerine başka bir sunucuya iletir biridir.</span><span class="sxs-lookup"><span data-stu-id="18e60-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="18e60-125">Ters proxy genellikle rastgele istemcileri adına sabit bir hedef iletir.</span><span class="sxs-lookup"><span data-stu-id="18e60-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="18e60-126">Bu kılavuzda, Apache Kestrel ASP.NET Core uygulaması ettiğini aynı sunucu üzerinde çalışan ters proxy olarak yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="18e60-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="18e60-127">Ters proxy tarafından istekleri iletilir çünkü [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="18e60-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="18e60-128">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` bu yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri doğru çalışması için başlık.</span><span class="sxs-lookup"><span data-stu-id="18e60-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="18e60-129">Kimlik doğrulaması, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi bir düzen bağlı olduğu herhangi bir bileşeni çağrılırken iletilen üstbilgileri Ara sonra yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="18e60-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="18e60-130">Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri ara yazılım çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="18e60-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="18e60-131">Bu sıralama, iletilen üst bilgi bağlı olan ara yazılım işleme için üstbilgi değerlerini tüketebileceği sağlar.</span><span class="sxs-lookup"><span data-stu-id="18e60-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="18e60-132">Çağırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yönteminde `Startup.Configure` çağırmadan önce <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> veya benzer kimlik doğrulaması düzeni ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="18e60-132">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="18e60-133">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="18e60-133">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="18e60-134">Çağırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yönteminde `Startup.Configure` çağırmadan önce <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> ve <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> veya benzer kimlik doğrulaması düzeni ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="18e60-134">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> and <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="18e60-135">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="18e60-135">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="18e60-136">Hayır ise <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilen ara yazılımıyla iletmek için varsayılan başlıkları `None`.</span><span class="sxs-lookup"><span data-stu-id="18e60-136">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="18e60-137">Geri döngü adreslerinde çalıştıran proxy'leri (127.0.0.0/8, [:: 1]), standart localhost adresi (127.0.0.1) dahil olmak üzere, varsayılan olarak güvenilir.</span><span class="sxs-lookup"><span data-stu-id="18e60-137">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="18e60-138">Diğer güvenilen proxy'leri veya kuruluş tanıtıcı istekler Internet ile web sunucusu arasında ağ eklerseniz bunları listesine <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> veya <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> ile <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="18e60-138">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="18e60-139">Aşağıdaki örnekte güvenilen Ara sunucu IP adresi 10.0.0.100 iletilen üstbilgileri ara yazılımı için ekler `KnownProxies` içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="18e60-139">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="18e60-140">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="18e60-140">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="18e60-141">Apache yükleyin</span><span class="sxs-lookup"><span data-stu-id="18e60-141">Install Apache</span></span>

<span data-ttu-id="18e60-142">CentOS paketlerinin en son kararlı sürümlerine güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="18e60-142">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="18e60-143">Apache web sunucusunun tek bir CentOS üzerinde yükleme `yum` komutu:</span><span class="sxs-lookup"><span data-stu-id="18e60-143">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="18e60-144">Komutu çalıştırdıktan sonra çıktı örneği:</span><span class="sxs-lookup"><span data-stu-id="18e60-144">Sample output after running the command:</span></span>

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
> <span data-ttu-id="18e60-145">CentOS 7 sürümü 64-bit olduğundan bu örnekte, çıktı httpd.86_64 yansıtır.</span><span class="sxs-lookup"><span data-stu-id="18e60-145">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="18e60-146">Apache yüklendiği doğrulamak için çalıştırın `whereis httpd` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="18e60-146">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="18e60-147">Apache yapılandırın</span><span class="sxs-lookup"><span data-stu-id="18e60-147">Configure Apache</span></span>

<span data-ttu-id="18e60-148">Yapılandırma dosyaları için Apache içinde bulunduğu `/etc/httpd/conf.d/` dizin.</span><span class="sxs-lookup"><span data-stu-id="18e60-148">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="18e60-149">Herhangi bir dosya ile *.conf* uzantı modülü yapılandırma dosyalarında yanı sıra alfabetik sırayla işlenir `/etc/httpd/conf.modules.d/`, modülleri yüklemek gerekli dosyaları içeren herhangi bir yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="18e60-149">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="18e60-150">Adlı bir yapılandırma dosyası oluşturma *helloapp.conf*, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="18e60-150">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

<span data-ttu-id="18e60-151">`VirtualHost` Blok bir sunucuda bir veya daha fazla dosya içinde birden çok kez görünebilir.</span><span class="sxs-lookup"><span data-stu-id="18e60-151">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="18e60-152">Önceki yapılandırma dosyasında Apache 80 numaralı bağlantı noktasında ortak trafiği kabul eder.</span><span class="sxs-lookup"><span data-stu-id="18e60-152">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="18e60-153">Etki alanı `www.example.com` hizmet verilen ve `*.example.com` diğer çözümler için aynı Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="18e60-153">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="18e60-154">Bkz: [sanal ana bilgisayar adı tabanlı destek](https://httpd.apache.org/docs/current/vhosts/name-based.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="18e60-154">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="18e60-155">İstekleri kök 127.0.0.1 server örneğinin 5000 numaralı bağlantı noktasına taşınır.</span><span class="sxs-lookup"><span data-stu-id="18e60-155">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="18e60-156">Çift yönlü iletişim için `ProxyPass` ve `ProxyPassReverse` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="18e60-156">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="18e60-157">Kestrel'i'nın IP/bağlantı noktasını değiştirmek için bkz [Kestrel: Uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="18e60-157">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="18e60-158">Uygun belirtmek için hata [ServerName yönergesi](https://httpd.apache.org/docs/current/mod/core.html#servername) içinde **VirtualHost** blok uygulamanıza güvenlik açıklarını kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="18e60-158">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="18e60-159">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanını denetimi bu güvenlik riski yoktur (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="18e60-159">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="18e60-160">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="18e60-160">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="18e60-161">Günlüğe kaydetme, başına yapılandırılabilir `VirtualHost` kullanarak `ErrorLog` ve `CustomLog` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="18e60-161">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="18e60-162">`ErrorLog` Burada sunucu günlüklerine hataları, konum ve `CustomLog` dosya adı ve günlük dosyası biçimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="18e60-162">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="18e60-163">Bu durumda, burada isteği bilgileri günlüğe budur.</span><span class="sxs-lookup"><span data-stu-id="18e60-163">In this case, this is where request information is logged.</span></span> <span data-ttu-id="18e60-164">Her istek için bir satır vardır.</span><span class="sxs-lookup"><span data-stu-id="18e60-164">There's one line for each request.</span></span>

<span data-ttu-id="18e60-165">Dosyayı kaydedin ve test yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="18e60-165">Save the file and test the configuration.</span></span> <span data-ttu-id="18e60-166">Her şeyi geçerse, yanıt olmalıdır `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="18e60-166">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="18e60-167">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="18e60-167">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="18e60-168">Uygulamayı izleme</span><span class="sxs-lookup"><span data-stu-id="18e60-168">Monitor the app</span></span>

<span data-ttu-id="18e60-169">Apache, artık yapılan isteklerini iletmek için Kurulum `http://localhost:80` sırasında Kestrel üzerinde çalışan ASP.NET Core uygulaması için `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="18e60-169">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="18e60-170">Ancak, Apache Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="18e60-170">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="18e60-171">Kullanım *systemd* başlatmak ve temel alınan web uygulamasını izleme için bir hizmet dosya oluşturun.</span><span class="sxs-lookup"><span data-stu-id="18e60-171">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="18e60-172">*systemd* başlatılmasını, durdurmasını ve işlemlerini yönetme için çok sayıda güçlü özellikler sağlar init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="18e60-172">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="18e60-173">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="18e60-173">Create the service file</span></span>

<span data-ttu-id="18e60-174">Hizmet tanım dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="18e60-174">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="18e60-175">Uygulama için bir örnek hizmeti dosyası:</span><span class="sxs-lookup"><span data-stu-id="18e60-175">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="18e60-176">Kullanıcının *apache* kullanılmayan yapılandırmaya göre kullanıcı ilk oluşturulmalı ve uygun dosyaları sahipliğini verilen.</span><span class="sxs-lookup"><span data-stu-id="18e60-176">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="18e60-177">Kullanım `TimeoutStopSec` uygulama ilk kesme sinyallerini aldığı sonra kapatmak beklenecek süre yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="18e60-177">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="18e60-178">Uygulama bu dönemde değil kapatırsanız SIGKILL uygulamayı sonlandırmak için verilir.</span><span class="sxs-lookup"><span data-stu-id="18e60-178">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="18e60-179">Unitless saniye değer sağlayın (örneğin, `150`), bir zaman aralığı değeri (örneğin, `2min 30s`), veya `infinity` zaman aşımı devre dışı bırakmak için.</span><span class="sxs-lookup"><span data-stu-id="18e60-179">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="18e60-180">`TimeoutStopSec` değerini varsayılan olarak `DefaultTimeoutStopSec` Yöneticisi yapılandırma dosyasında (*systemd system.conf*, *system.conf.d*, *systemd user.conf*,  *User.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="18e60-180">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="18e60-181">Çoğu dağıtımlar için varsayılan zaman aşımı değeri 90 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="18e60-181">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="18e60-182">Ortam değişkenlerini okumak yapılandırma sağlayıcıları için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçınılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="18e60-182">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="18e60-183">Yapılandırma dosyasında kullanmak için düzgün bir şekilde atlanan bir değer oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="18e60-183">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="18e60-184">Dosyayı kaydedin ve hizmeti etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="18e60-184">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="18e60-185">Hizmeti başlatın ve çalıştığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="18e60-185">Start the service and verify that it's running:</span></span>

```bash
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="18e60-186">İle yönetilen Kestrel ve yapılandırılmış bir ters proxy *systemd*, web uygulaması, tam olarak yapılandırılır ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="18e60-186">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="18e60-187">Yanıt üst bilgilerini inceleyerek **sunucu** üst bilgisi gösterir ASP.NET Core uygulaması Kestrel tarafından sunulur:</span><span class="sxs-lookup"><span data-stu-id="18e60-187">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="18e60-188">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="18e60-188">View logs</span></span>

<span data-ttu-id="18e60-189">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir *systemd*, olaylar ve işlemleri için merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="18e60-189">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="18e60-190">Ancak, bu günlük girişlerini tüm hizmetleri ve işlemleri tarafından yönetilen içerir *systemd*.</span><span class="sxs-lookup"><span data-stu-id="18e60-190">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="18e60-191">Görüntülenecek `kestrel-helloapp.service`-belirli öğeler, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="18e60-191">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="18e60-192">Zaman filtresi uygulamak için zaman seçenekleri ile komutu belirtin.</span><span class="sxs-lookup"><span data-stu-id="18e60-192">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="18e60-193">Örneğin, `--since today` geçerli gün için filtre uygulamak veya `--until 1 hour ago` önceki saat girişlerini görmek için.</span><span class="sxs-lookup"><span data-stu-id="18e60-193">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="18e60-194">Daha fazla bilgi için [journalctl man sayfası](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="18e60-194">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="18e60-195">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="18e60-195">Data protection</span></span>

<span data-ttu-id="18e60-196">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index)kimlik doğrulaması ara yazılımı (örneğin, tanımlama bilgisi Ara) dahil olmak üzere ve siteler arası istek sahteciliği (CSRF) korumaları.</span><span class="sxs-lookup"><span data-stu-id="18e60-196">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="18e60-197">Bir kalıcı oluşturmak için veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="18e60-197">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="18e60-198">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="18e60-198">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="18e60-199">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="18e60-199">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="18e60-200">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="18e60-200">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="18e60-201">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="18e60-201">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="18e60-202">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="18e60-202">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="18e60-203">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="18e60-203">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="18e60-204">Kalıcı hale getirmek ve anahtar halkası şifrelemek için veri korumayı yapılandırmak için bkz:</span><span class="sxs-lookup"><span data-stu-id="18e60-204">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="18e60-205">Bir uygulamanın güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="18e60-205">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="18e60-206">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="18e60-206">Configure firewall</span></span>

<span data-ttu-id="18e60-207">*Firewalld* ağ bölgeleri için destek Güvenlik Duvarı'nı yönetmek için bir dinamik arka plan programı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="18e60-207">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="18e60-208">Hala bağlantı noktaları ve paket filtreleme iptables tarafından yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="18e60-208">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="18e60-209">*Firewalld* varsayılan olarak yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="18e60-209">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="18e60-210">`yum` paketi yüklemek veya yüklü doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="18e60-210">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="18e60-211">Kullanım `firewalld` yalnızca uygulama için gerekli bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="18e60-211">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="18e60-212">Bu durumda, bağlantı noktası 80 ve 443 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="18e60-212">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="18e60-213">Aşağıdaki komutlar, bağlantı noktaları 80 ve 443'ü açmak için kalıcı olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="18e60-213">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="18e60-214">Güvenlik Duvarı ayarlarını yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="18e60-214">Reload the firewall settings.</span></span> <span data-ttu-id="18e60-215">Kullanılabilir hizmetleri ve bağlantı noktalarını varsayılan bölgede denetleyin.</span><span class="sxs-lookup"><span data-stu-id="18e60-215">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="18e60-216">Seçenekleri kullanılabilir inceleyerek `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="18e60-216">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="https-configuration"></a><span data-ttu-id="18e60-217">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="18e60-217">HTTPS configuration</span></span>

<span data-ttu-id="18e60-218">HTTPS için Apache yapılandırmak için *mod_ssl* modülü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="18e60-218">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="18e60-219">Zaman *httpd* modülü yüklendi *mod_ssl* modülü de yüklendi.</span><span class="sxs-lookup"><span data-stu-id="18e60-219">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="18e60-220">Yüklü değildi kullanırsanız `yum` yapılandırmanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="18e60-220">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="18e60-221">HTTPS zorlama için yükleme `mod_rewrite` etkinleştirme URL yeniden yazma Modülü:</span><span class="sxs-lookup"><span data-stu-id="18e60-221">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="18e60-222">Değiştirme *helloapp.conf* URL yeniden yazma etkinleştirmek ve bağlantı noktası 443 üzerinden iletişimi güvenli hale getirmek için dosya:</span><span class="sxs-lookup"><span data-stu-id="18e60-222">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="18e60-223">Bu örnek, yerel olarak oluşturulan bir sertifika kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="18e60-223">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="18e60-224">**SSLCertificateFile** etki alanı adının birincil sertifika dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="18e60-224">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="18e60-225">**SSLCertificateKeyFile** CSR oluşturulurken anahtar dosyası oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="18e60-225">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="18e60-226">**SSLCertificateChainFile** ara sertifika dosyasını (varsa) olmalıdır sertifika yetkilisi tarafından sağlandı.</span><span class="sxs-lookup"><span data-stu-id="18e60-226">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="18e60-227">Dosyayı kaydedin ve test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="18e60-227">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="18e60-228">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="18e60-228">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="18e60-229">Ek Apache öneriler</span><span class="sxs-lookup"><span data-stu-id="18e60-229">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="18e60-230">Ek üst bilgiler</span><span class="sxs-lookup"><span data-stu-id="18e60-230">Additional headers</span></span>

<span data-ttu-id="18e60-231">Kötü amaçlı saldırılara karşı güvenli hale getirmek için ya da değiştirildiğinde veya birkaç üstbilgileri vardır.</span><span class="sxs-lookup"><span data-stu-id="18e60-231">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="18e60-232">Emin `mod_headers` modülünün yüklü:</span><span class="sxs-lookup"><span data-stu-id="18e60-232">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="18e60-233">Apache clickjacking saldırılarına karşı güvenli</span><span class="sxs-lookup"><span data-stu-id="18e60-233">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="18e60-234">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)olarak da bilinen bir *UI redress saldırı*, bir kötü amaçlı bir Web sitesi ziyaretçi yere sağladı daha şu anda ziyaret ettiğiniz bir bağlantı veya başka bir sayfaya düğmesine tıklamak saldırıdır.</span><span class="sxs-lookup"><span data-stu-id="18e60-234">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="18e60-235">Kullanım `X-FRAME-OPTIONS` sitesini güvenli hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="18e60-235">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="18e60-236">Clickjacking saldırıları azaltmak için:</span><span class="sxs-lookup"><span data-stu-id="18e60-236">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="18e60-237">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="18e60-237">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="18e60-238">Satır Ekle `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="18e60-238">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="18e60-239">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="18e60-239">Save the file.</span></span>
1. <span data-ttu-id="18e60-240">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="18e60-240">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="18e60-241">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="18e60-241">MIME-type sniffing</span></span>

<span data-ttu-id="18e60-242">`X-Content-Type-Options` Üstbilgi engeller Internet Explorer'dan *MIME algılaması* (bir dosyanın belirleme `Content-Type` dosyasının içeriğinden).</span><span class="sxs-lookup"><span data-stu-id="18e60-242">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="18e60-243">Sunucu ayarlarsa `Content-Type` başlığına `text/html` ile `nosniff` seçenek kümesi, Internet Explorer içeriği olarak işler `text/html` dosyanın içeriği ne olursa olsun.</span><span class="sxs-lookup"><span data-stu-id="18e60-243">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="18e60-244">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="18e60-244">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="18e60-245">Satır Ekle `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="18e60-245">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="18e60-246">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="18e60-246">Save the file.</span></span> <span data-ttu-id="18e60-247">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="18e60-247">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="18e60-248">YükDengeleme</span><span class="sxs-lookup"><span data-stu-id="18e60-248">Load Balancing</span></span>

<span data-ttu-id="18e60-249">Bu örnek, Kurulum ve aynı örnek makinede Apache CentOS 7 ve Kestrel yapılandırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="18e60-249">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="18e60-250">Bir tek hata noktası olmaması için; kullanarak *mod_proxy_balancer* ve değiştirme **VirtualHost** Apache proxy sunucunun arkasındaki web uygulamaları birden çok örneğini yönetmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="18e60-250">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="18e60-251">Yapılandırma dosyasında ek bir örneği aşağıda gösterilen `helloapp` çalıştırılacak bağlantı noktası 5001 kadar ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="18e60-251">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="18e60-252">*Proxy* bölümü ayarlanmış iki üyeli dengeleyici yapılandırmasına sahip Yük Dengelemesi *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="18e60-252">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
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
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="18e60-253">Oran sınırları</span><span class="sxs-lookup"><span data-stu-id="18e60-253">Rate Limits</span></span>

<span data-ttu-id="18e60-254">Kullanarak *mod_ratelimit*, içinde bulunan *httpd* modülü, bant genişliği istemcilerin olabilir sınırlı:</span><span class="sxs-lookup"><span data-stu-id="18e60-254">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="18e60-255">Örnek dosyası kök konumu altında 600 KB/sn olarak bant genişliği sınırları:</span><span class="sxs-lookup"><span data-stu-id="18e60-255">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="18e60-256">Uzun bir isteği üst bilgi alanları</span><span class="sxs-lookup"><span data-stu-id="18e60-256">Long request header fields</span></span>

<span data-ttu-id="18e60-257">Uygulama isteği üstbilgi alanlarını ayarlama (genellikle 8,190 bayt) proxy sunucunun varsayılan olarak izin verilenden daha uzun gerektiriyorsa, değerini ayarla [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) yönergesi.</span><span class="sxs-lookup"><span data-stu-id="18e60-257">If the app requires request header fields longer than permitted by the proxy server's default setting (typically 8,190 bytes), adjust the value of the [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive.</span></span> <span data-ttu-id="18e60-258">Uygulamak için değer senaryoya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="18e60-258">The value to apply is scenario-dependent.</span></span> <span data-ttu-id="18e60-259">Daha fazla bilgi için sunucunuzun belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="18e60-259">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="18e60-260">Varsayılan değer olan artırmaz `LimitRequestFieldSize` sürece gerekli.</span><span class="sxs-lookup"><span data-stu-id="18e60-260">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="18e60-261">(Taşma) arabellek taşması riskini artırır değeri artırmak ve kötü amaçlı kullanıcılar tarafından hizmet reddi (DoS) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="18e60-261">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18e60-262">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="18e60-262">Additional resources</span></span>

* [<span data-ttu-id="18e60-263">Linux üzerinde .NET Core önkoşulları</span><span class="sxs-lookup"><span data-stu-id="18e60-263">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
