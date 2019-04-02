---
title: ASP.NET Core Apache ile Linux'ta barındırma
author: spboyer
description: Apache CentOS, ters Ara sunucu olarak Kestrel üzerinde çalışan ASP.NET Core web uygulaması için HTTP trafiğini yönlendirmek için nasıl kuracağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: spboyer
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/linux-apache
ms.openlocfilehash: fbdfe9c19f3cbf6d12678187bb07e58e82395d2f
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750646"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="52c66-103">ASP.NET Core Apache ile Linux'ta barındırma</span><span class="sxs-lookup"><span data-stu-id="52c66-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="52c66-104">Tarafından [Shayne boyer'ın](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="52c66-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="52c66-105">Bu kılavuz kullanılarak nasıl ayarlanacağını öğrenin [Apache](https://httpd.apache.org/) ters Ara sunucu olarak [CentOS 7](https://www.centos.org/) üzerinde çalışan ASP.NET Core web uygulaması için HTTP trafiğini yönlendirmek için [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="52c66-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="52c66-106">[Mod_proxy uzantısı](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) ve ilgili modüller ters proxy sunucunun oluşturun.</span><span class="sxs-lookup"><span data-stu-id="52c66-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52c66-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="52c66-107">Prerequisites</span></span>

* <span data-ttu-id="52c66-108">Sudo ayrıcalıklarıyla standart kullanıcı hesabı ile CentOS 7 çalıştıran sunucu.</span><span class="sxs-lookup"><span data-stu-id="52c66-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="52c66-109">.NET Core çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="52c66-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="52c66-110">Ziyaret [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="52c66-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="52c66-111">En son Önizleme çalışma zamanı altında listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="52c66-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="52c66-112">Seçin ve CentOS/Oracle için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="52c66-112">Select and follow the instructions for CentOS/Oracle.</span></span>
* <span data-ttu-id="52c66-113">Mevcut bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="52c66-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="52c66-114">Yayımlama ve uygulamanın üzerine kopyalayın</span><span class="sxs-lookup"><span data-stu-id="52c66-114">Publish and copy over the app</span></span>

<span data-ttu-id="52c66-115">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="52c66-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="52c66-116">Uygulamayı yerel olarak çalıştırın ve güvenli bağlantı (HTTPS) yapılandırılmamış, aşağıdaki yaklaşımlardan birini benimseme:</span><span class="sxs-lookup"><span data-stu-id="52c66-116">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="52c66-117">Yerel güvenli bağlantılar işlemek üzere uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="52c66-117">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="52c66-118">Daha fazla bilgi için [HTTPS yapılandırma](#https-configuration) bölümü.</span><span class="sxs-lookup"><span data-stu-id="52c66-118">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="52c66-119">Kaldırma `https://localhost:5001` (varsa) öğesinden `applicationUrl` özelliğinde *Properties/launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="52c66-119">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="52c66-120">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulamayı paketlemek için geliştirme ortamından (örneğin, *bin/yayın/&lt;target_framework_moniker&gt;/ publish*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="52c66-120">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="52c66-121">Uygulama ayrıca olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) .NET Core çalışma zamanı sunucuda değil sağlamak isterseniz.</span><span class="sxs-lookup"><span data-stu-id="52c66-121">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="52c66-122">ASP.NET Core uygulaması (örneğin, SCP, SFTP) kuruluşun iş akışınıza tümleştirir bir aracını kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="52c66-122">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="52c66-123">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *www/var/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="52c66-123">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="52c66-124">Üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="52c66-124">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="52c66-125">Bir proxy sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="52c66-125">Configure a proxy server</span></span>

<span data-ttu-id="52c66-126">Ters proxy hizmet dinamik web uygulamaları için ortak bir kurulum var.</span><span class="sxs-lookup"><span data-stu-id="52c66-126">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="52c66-127">Ters proxy, HTTP isteği sonlandırır ve ASP.NET uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="52c66-127">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="52c66-128">Bir proxy sunucusu, istemci istekleri istekleri kendisi yerine getirmesini yerine başka bir sunucuya iletir biridir.</span><span class="sxs-lookup"><span data-stu-id="52c66-128">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="52c66-129">Ters proxy genellikle rastgele istemcileri adına sabit bir hedef iletir.</span><span class="sxs-lookup"><span data-stu-id="52c66-129">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="52c66-130">Bu kılavuzda, Apache Kestrel ASP.NET Core uygulaması ettiğini aynı sunucu üzerinde çalışan ters proxy olarak yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="52c66-130">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="52c66-131">Ters proxy tarafından istekleri iletilir çünkü [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="52c66-131">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="52c66-132">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` bu yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri doğru çalışması için başlık.</span><span class="sxs-lookup"><span data-stu-id="52c66-132">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="52c66-133">Kimlik doğrulaması, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi bir düzen bağlı olduğu herhangi bir bileşeni çağrılırken iletilen üstbilgileri Ara sonra yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="52c66-133">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="52c66-134">Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri ara yazılım çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="52c66-134">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="52c66-135">Bu sıralama, iletilen üst bilgi bağlı olan ara yazılım işleme için üstbilgi değerlerini tüketebileceği sağlar.</span><span class="sxs-lookup"><span data-stu-id="52c66-135">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

<span data-ttu-id="52c66-136">Çağırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yönteminde `Startup.Configure` çağırmadan önce <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> veya benzer kimlik doğrulaması düzeni ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="52c66-136">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="52c66-137">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="52c66-137">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="52c66-138">Hayır ise <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilen ara yazılımıyla iletmek için varsayılan başlıkları `None`.</span><span class="sxs-lookup"><span data-stu-id="52c66-138">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="52c66-139">Geri döngü adreslerinde çalıştıran proxy'leri (127.0.0.0/8, [:: 1]), standart localhost adresi (127.0.0.1) dahil olmak üzere, varsayılan olarak güvenilir.</span><span class="sxs-lookup"><span data-stu-id="52c66-139">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="52c66-140">Diğer güvenilen proxy'leri veya kuruluş tanıtıcı istekler Internet ile web sunucusu arasında ağ eklerseniz bunları listesine <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> veya <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> ile <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="52c66-140">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="52c66-141">Aşağıdaki örnekte güvenilen Ara sunucu IP adresi 10.0.0.100 iletilen üstbilgileri ara yazılımı için ekler `KnownProxies` içinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="52c66-141">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="52c66-142">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="52c66-142">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="52c66-143">Apache yükleyin</span><span class="sxs-lookup"><span data-stu-id="52c66-143">Install Apache</span></span>

<span data-ttu-id="52c66-144">CentOS paketlerinin en son kararlı sürümlerine güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="52c66-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="52c66-145">Apache web sunucusunun tek bir CentOS üzerinde yükleme `yum` komutu:</span><span class="sxs-lookup"><span data-stu-id="52c66-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="52c66-146">Komutu çalıştırdıktan sonra çıktı örneği:</span><span class="sxs-lookup"><span data-stu-id="52c66-146">Sample output after running the command:</span></span>

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
> <span data-ttu-id="52c66-147">CentOS 7 sürümü 64-bit olduğundan bu örnekte, çıktı httpd.86_64 yansıtır.</span><span class="sxs-lookup"><span data-stu-id="52c66-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="52c66-148">Apache yüklendiği doğrulamak için çalıştırın `whereis httpd` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="52c66-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="52c66-149">Apache yapılandırın</span><span class="sxs-lookup"><span data-stu-id="52c66-149">Configure Apache</span></span>

<span data-ttu-id="52c66-150">Yapılandırma dosyaları için Apache içinde bulunduğu `/etc/httpd/conf.d/` dizin.</span><span class="sxs-lookup"><span data-stu-id="52c66-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="52c66-151">Herhangi bir dosya ile *.conf* uzantı modülü yapılandırma dosyalarında yanı sıra alfabetik sırayla işlenir `/etc/httpd/conf.modules.d/`, modülleri yüklemek gerekli dosyaları içeren herhangi bir yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="52c66-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="52c66-152">Adlı bir yapılandırma dosyası oluşturma *helloapp.conf*, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="52c66-152">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

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

<span data-ttu-id="52c66-153">`VirtualHost` Blok bir sunucuda bir veya daha fazla dosya içinde birden çok kez görünebilir.</span><span class="sxs-lookup"><span data-stu-id="52c66-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="52c66-154">Önceki yapılandırma dosyasında Apache 80 numaralı bağlantı noktasında ortak trafiği kabul eder.</span><span class="sxs-lookup"><span data-stu-id="52c66-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="52c66-155">Etki alanı `www.example.com` hizmet verilen ve `*.example.com` diğer çözümler için aynı Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="52c66-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="52c66-156">Bkz: [sanal ana bilgisayar adı tabanlı destek](https://httpd.apache.org/docs/current/vhosts/name-based.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="52c66-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="52c66-157">İstekleri kök 127.0.0.1 server örneğinin 5000 numaralı bağlantı noktasına taşınır.</span><span class="sxs-lookup"><span data-stu-id="52c66-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="52c66-158">Çift yönlü iletişim için `ProxyPass` ve `ProxyPassReverse` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="52c66-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="52c66-159">Kestrel'i'nın IP/bağlantı noktasını değiştirmek için bkz [Kestrel: Uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="52c66-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="52c66-160">Uygun belirtmek için hata [ServerName yönergesi](https://httpd.apache.org/docs/current/mod/core.html#servername) içinde **VirtualHost** blok uygulamanıza güvenlik açıklarını kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="52c66-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="52c66-161">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanını denetimi bu güvenlik riski yoktur (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="52c66-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="52c66-162">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="52c66-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="52c66-163">Günlüğe kaydetme, başına yapılandırılabilir `VirtualHost` kullanarak `ErrorLog` ve `CustomLog` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="52c66-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="52c66-164">`ErrorLog` Burada sunucu günlüklerine hataları, konum ve `CustomLog` dosya adı ve günlük dosyası biçimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="52c66-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="52c66-165">Bu durumda, burada isteği bilgileri günlüğe budur.</span><span class="sxs-lookup"><span data-stu-id="52c66-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="52c66-166">Her istek için bir satır vardır.</span><span class="sxs-lookup"><span data-stu-id="52c66-166">There's one line for each request.</span></span>

<span data-ttu-id="52c66-167">Dosyayı kaydedin ve test yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="52c66-167">Save the file and test the configuration.</span></span> <span data-ttu-id="52c66-168">Her şeyi geçerse, yanıt olmalıdır `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="52c66-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="52c66-169">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="52c66-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="52c66-170">Uygulamayı izleme</span><span class="sxs-lookup"><span data-stu-id="52c66-170">Monitor the app</span></span>

<span data-ttu-id="52c66-171">Apache, artık yapılan isteklerini iletmek için Kurulum `http://localhost:80` sırasında Kestrel üzerinde çalışan ASP.NET Core uygulaması için `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="52c66-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="52c66-172">Ancak, Apache Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="52c66-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="52c66-173">Kullanım *systemd* başlatmak ve temel alınan web uygulamasını izleme için bir hizmet dosya oluşturun.</span><span class="sxs-lookup"><span data-stu-id="52c66-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="52c66-174">*systemd* başlatılmasını, durdurmasını ve işlemlerini yönetme için çok sayıda güçlü özellikler sağlar init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="52c66-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="52c66-175">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="52c66-175">Create the service file</span></span>

<span data-ttu-id="52c66-176">Hizmet tanım dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="52c66-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="52c66-177">Uygulama için bir örnek hizmeti dosyası:</span><span class="sxs-lookup"><span data-stu-id="52c66-177">An example service file for the app:</span></span>

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

<span data-ttu-id="52c66-178">Kullanıcının *apache* kullanılmayan yapılandırmaya göre kullanıcı ilk oluşturulmalı ve uygun dosyaları sahipliğini verilen.</span><span class="sxs-lookup"><span data-stu-id="52c66-178">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="52c66-179">Kullanım `TimeoutStopSec` uygulama ilk kesme sinyallerini aldığı sonra kapatmak beklenecek süre yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="52c66-179">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="52c66-180">Uygulama bu dönemde değil kapatırsanız SIGKILL uygulamayı sonlandırmak için verilir.</span><span class="sxs-lookup"><span data-stu-id="52c66-180">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="52c66-181">Unitless saniye değer sağlayın (örneğin, `150`), bir zaman aralığı değeri (örneğin, `2min 30s`), veya `infinity` zaman aşımı devre dışı bırakmak için.</span><span class="sxs-lookup"><span data-stu-id="52c66-181">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="52c66-182">`TimeoutStopSec` değerini varsayılan olarak `DefaultTimeoutStopSec` Yöneticisi yapılandırma dosyasında (*systemd system.conf*, *system.conf.d*, *systemd user.conf*, \* User.conf.d\*).</span><span class="sxs-lookup"><span data-stu-id="52c66-182">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="52c66-183">Çoğu dağıtımlar için varsayılan zaman aşımı değeri 90 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="52c66-183">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="52c66-184">Ortam değişkenlerini okumak yapılandırma sağlayıcıları için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçınılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="52c66-184">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="52c66-185">Yapılandırma dosyasında kullanmak için düzgün bir şekilde atlanan bir değer oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="52c66-185">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="52c66-186">Dosyayı kaydedin ve hizmeti etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="52c66-186">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="52c66-187">Hizmeti başlatın ve çalıştığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="52c66-187">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="52c66-188">İle yönetilen Kestrel ve yapılandırılmış bir ters proxy *systemd*, web uygulaması, tam olarak yapılandırılır ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="52c66-188">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="52c66-189">Yanıt üst bilgilerini inceleyerek **sunucu** üst bilgisi gösterir ASP.NET Core uygulaması Kestrel tarafından sunulur:</span><span class="sxs-lookup"><span data-stu-id="52c66-189">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="52c66-190">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="52c66-190">View logs</span></span>

<span data-ttu-id="52c66-191">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir *systemd*, olaylar ve işlemleri için merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="52c66-191">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="52c66-192">Ancak, bu günlük girişlerini tüm hizmetleri ve işlemleri tarafından yönetilen içerir *systemd*.</span><span class="sxs-lookup"><span data-stu-id="52c66-192">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="52c66-193">Görüntülenecek `kestrel-helloapp.service`-belirli öğeler, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="52c66-193">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="52c66-194">Zaman filtresi uygulamak için zaman seçenekleri ile komutu belirtin.</span><span class="sxs-lookup"><span data-stu-id="52c66-194">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="52c66-195">Örneğin, `--since today` geçerli gün için filtre uygulamak veya `--until 1 hour ago` önceki saat girişlerini görmek için.</span><span class="sxs-lookup"><span data-stu-id="52c66-195">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="52c66-196">Daha fazla bilgi için [journalctl man sayfası](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="52c66-196">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="52c66-197">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="52c66-197">Data protection</span></span>

<span data-ttu-id="52c66-198">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index)kimlik doğrulaması ara yazılımı (örneğin, tanımlama bilgisi Ara) dahil olmak üzere ve siteler arası istek sahteciliği (CSRF) korumaları.</span><span class="sxs-lookup"><span data-stu-id="52c66-198">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="52c66-199">Bir kalıcı oluşturmak için veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="52c66-199">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="52c66-200">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="52c66-200">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="52c66-201">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="52c66-201">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="52c66-202">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="52c66-202">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="52c66-203">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="52c66-203">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="52c66-204">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="52c66-204">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="52c66-205">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="52c66-205">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="52c66-206">Kalıcı hale getirmek ve anahtar halkası şifrelemek için veri korumayı yapılandırmak için bkz:</span><span class="sxs-lookup"><span data-stu-id="52c66-206">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="52c66-207">Bir uygulamanın güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="52c66-207">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="52c66-208">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="52c66-208">Configure firewall</span></span>

<span data-ttu-id="52c66-209">*Firewalld* ağ bölgeleri için destek Güvenlik Duvarı'nı yönetmek için bir dinamik arka plan programı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="52c66-209">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="52c66-210">Hala bağlantı noktaları ve paket filtreleme iptables tarafından yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="52c66-210">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="52c66-211">*Firewalld* varsayılan olarak yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="52c66-211">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="52c66-212">`yum` paketi yüklemek veya yüklü doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="52c66-212">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="52c66-213">Kullanım `firewalld` yalnızca uygulama için gerekli bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="52c66-213">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="52c66-214">Bu durumda, bağlantı noktası 80 ve 443 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="52c66-214">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="52c66-215">Aşağıdaki komutlar, bağlantı noktaları 80 ve 443'ü açmak için kalıcı olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="52c66-215">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="52c66-216">Güvenlik Duvarı ayarlarını yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="52c66-216">Reload the firewall settings.</span></span> <span data-ttu-id="52c66-217">Kullanılabilir hizmetleri ve bağlantı noktalarını varsayılan bölgede denetleyin.</span><span class="sxs-lookup"><span data-stu-id="52c66-217">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="52c66-218">Seçenekleri kullanılabilir inceleyerek `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="52c66-218">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="https-configuration"></a><span data-ttu-id="52c66-219">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="52c66-219">HTTPS configuration</span></span>

<span data-ttu-id="52c66-220">**Güvenli (HTTPS) yerel bağlantılar için uygulamayı yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="52c66-220">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="52c66-221">[Çalıştırma dotnet](/dotnet/core/tools/dotnet-run) komutunu kullanan uygulamanın *Properties/launchSettings.json* uygulama tarafından sağlanan URL'leri dinleyecek şekilde yapılandıran dosya `applicationUrl` özelliği (örneğin, `https://localhost:5001;http://localhost:5000`) .</span><span class="sxs-lookup"><span data-stu-id="52c66-221">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="52c66-222">Geliştirme için bir sertifika kullanmak üzere uygulamayı yapılandırır `dotnet run` komut veya şunlardan birini yaklaşıyor geliştirme ortamı (F5 veya Visual Studio code'da Ctrl + F5) kullanarak:</span><span class="sxs-lookup"><span data-stu-id="52c66-222">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="52c66-223">[Varsayılan Sertifika yapılandırmasından değiştirin](xref:fundamentals/servers/kestrel#configuration) (*önerilen*)</span><span class="sxs-lookup"><span data-stu-id="52c66-223">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="52c66-224">KestrelServerOptions.ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="52c66-224">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="52c66-225">**Güvenli (HTTPS) istemci bağlantıları için ters proxy ayarlarını yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="52c66-225">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

<span data-ttu-id="52c66-226">HTTPS için Apache yapılandırmak için *mod_ssl* modülü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="52c66-226">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="52c66-227">Zaman *httpd* modülü yüklendi *mod_ssl* modülü de yüklendi.</span><span class="sxs-lookup"><span data-stu-id="52c66-227">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="52c66-228">Yüklü değildi kullanırsanız `yum` yapılandırmanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="52c66-228">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="52c66-229">HTTPS zorlama için yükleme `mod_rewrite` etkinleştirme URL yeniden yazma Modülü:</span><span class="sxs-lookup"><span data-stu-id="52c66-229">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="52c66-230">Değiştirme *helloapp.conf* URL yeniden yazma etkinleştirmek ve bağlantı noktası 443 üzerinden iletişimi güvenli hale getirmek için dosya:</span><span class="sxs-lookup"><span data-stu-id="52c66-230">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="52c66-231">Bu örnek, yerel olarak oluşturulan bir sertifika kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="52c66-231">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="52c66-232">**SSLCertificateFile** etki alanı adının birincil sertifika dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="52c66-232">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="52c66-233">**SSLCertificateKeyFile** CSR oluşturulurken anahtar dosyası oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="52c66-233">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="52c66-234">**SSLCertificateChainFile** ara sertifika dosyasını (varsa) olmalıdır sertifika yetkilisi tarafından sağlandı.</span><span class="sxs-lookup"><span data-stu-id="52c66-234">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="52c66-235">Dosyayı kaydedin ve test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="52c66-235">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="52c66-236">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="52c66-236">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="52c66-237">Ek Apache öneriler</span><span class="sxs-lookup"><span data-stu-id="52c66-237">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="52c66-238">Ek üst bilgiler</span><span class="sxs-lookup"><span data-stu-id="52c66-238">Additional headers</span></span>

<span data-ttu-id="52c66-239">Kötü amaçlı saldırılara karşı güvenli hale getirmek için ya da değiştirildiğinde veya birkaç üstbilgileri vardır.</span><span class="sxs-lookup"><span data-stu-id="52c66-239">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="52c66-240">Emin `mod_headers` modülünün yüklü:</span><span class="sxs-lookup"><span data-stu-id="52c66-240">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="52c66-241">Apache clickjacking saldırılarına karşı güvenli</span><span class="sxs-lookup"><span data-stu-id="52c66-241">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="52c66-242">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)olarak da bilinen bir *UI redress saldırı*, bir kötü amaçlı bir Web sitesi ziyaretçi yere sağladı daha şu anda ziyaret ettiğiniz bir bağlantı veya başka bir sayfaya düğmesine tıklamak saldırıdır.</span><span class="sxs-lookup"><span data-stu-id="52c66-242">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="52c66-243">Kullanım `X-FRAME-OPTIONS` sitesini güvenli hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="52c66-243">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="52c66-244">Clickjacking saldırıları azaltmak için:</span><span class="sxs-lookup"><span data-stu-id="52c66-244">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="52c66-245">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="52c66-245">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="52c66-246">Satır Ekle `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="52c66-246">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="52c66-247">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="52c66-247">Save the file.</span></span>
1. <span data-ttu-id="52c66-248">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="52c66-248">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="52c66-249">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="52c66-249">MIME-type sniffing</span></span>

<span data-ttu-id="52c66-250">`X-Content-Type-Options` Üstbilgi engeller Internet Explorer'dan *MIME algılaması* (bir dosyanın belirleme `Content-Type` dosyasının içeriğinden).</span><span class="sxs-lookup"><span data-stu-id="52c66-250">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="52c66-251">Sunucu ayarlarsa `Content-Type` başlığına `text/html` ile `nosniff` seçenek kümesi, Internet Explorer içeriği olarak işler `text/html` dosyanın içeriği ne olursa olsun.</span><span class="sxs-lookup"><span data-stu-id="52c66-251">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="52c66-252">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="52c66-252">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="52c66-253">Satır Ekle `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="52c66-253">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="52c66-254">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="52c66-254">Save the file.</span></span> <span data-ttu-id="52c66-255">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="52c66-255">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="52c66-256">YükDengeleme</span><span class="sxs-lookup"><span data-stu-id="52c66-256">Load Balancing</span></span>

<span data-ttu-id="52c66-257">Bu örnek, Kurulum ve aynı örnek makinede Apache CentOS 7 ve Kestrel yapılandırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="52c66-257">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="52c66-258">Bir tek hata noktası olmaması için; kullanarak *mod_proxy_balancer* ve değiştirme **VirtualHost** Apache proxy sunucunun arkasındaki web uygulamaları birden çok örneğini yönetmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="52c66-258">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="52c66-259">Yapılandırma dosyasında ek bir örneği aşağıda gösterilen `helloapp` çalıştırılacak bağlantı noktası 5001 kadar ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="52c66-259">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="52c66-260">*Proxy* bölümü ayarlanmış iki üyeli dengeleyici yapılandırmasına sahip Yük Dengelemesi *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="52c66-260">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="52c66-261">Oran sınırları</span><span class="sxs-lookup"><span data-stu-id="52c66-261">Rate Limits</span></span>

<span data-ttu-id="52c66-262">Kullanarak *mod_ratelimit*, içinde bulunan *httpd* modülü, bant genişliği istemcilerin olabilir sınırlı:</span><span class="sxs-lookup"><span data-stu-id="52c66-262">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="52c66-263">Örnek dosyası kök konumu altında 600 KB/sn olarak bant genişliği sınırları:</span><span class="sxs-lookup"><span data-stu-id="52c66-263">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="52c66-264">Uzun bir isteği üst bilgi alanları</span><span class="sxs-lookup"><span data-stu-id="52c66-264">Long request header fields</span></span>

<span data-ttu-id="52c66-265">Uygulama isteği üstbilgi alanlarını ayarlama (genellikle 8,190 bayt) proxy sunucunun varsayılan olarak izin verilenden daha uzun gerektiriyorsa, değerini ayarla [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) yönergesi.</span><span class="sxs-lookup"><span data-stu-id="52c66-265">If the app requires request header fields longer than permitted by the proxy server's default setting (typically 8,190 bytes), adjust the value of the [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive.</span></span> <span data-ttu-id="52c66-266">Uygulamak için değer senaryoya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="52c66-266">The value to apply is scenario-dependent.</span></span> <span data-ttu-id="52c66-267">Daha fazla bilgi için sunucunuzun belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="52c66-267">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="52c66-268">Varsayılan değer olan artırmaz `LimitRequestFieldSize` sürece gerekli.</span><span class="sxs-lookup"><span data-stu-id="52c66-268">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="52c66-269">(Taşma) arabellek taşması riskini artırır değeri artırmak ve kötü amaçlı kullanıcılar tarafından hizmet reddi (DoS) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="52c66-269">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52c66-270">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="52c66-270">Additional resources</span></span>

* [<span data-ttu-id="52c66-271">Linux üzerinde .NET Core önkoşulları</span><span class="sxs-lookup"><span data-stu-id="52c66-271">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
