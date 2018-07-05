---
title: ASP.NET Core Apache ile Linux'ta barındırma
description: Apache CentOS, ters Ara sunucu olarak Kestrel üzerinde çalışan ASP.NET Core web uygulaması için HTTP trafiğini yönlendirmek için nasıl kuracağınızı öğrenin.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: d02fbd82be37e6d67214a9a0bf5851662b577cb9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433980"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="89c31-103">ASP.NET Core Apache ile Linux'ta barındırma</span><span class="sxs-lookup"><span data-stu-id="89c31-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="89c31-104">Tarafından [Shayne boyer'ın](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="89c31-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="89c31-105">Bu kılavuz kullanılarak nasıl ayarlanacağını öğrenin [Apache](https://httpd.apache.org/) ters Ara sunucu olarak [CentOS 7](https://www.centos.org/) üzerinde çalışan ASP.NET Core web uygulaması için HTTP trafiğini yönlendirmek için [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="89c31-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="89c31-106">[Mod_proxy uzantısı](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) ve ilgili modüller ters proxy sunucunun oluşturun.</span><span class="sxs-lookup"><span data-stu-id="89c31-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89c31-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="89c31-107">Prerequisites</span></span>

1. <span data-ttu-id="89c31-108">Sudo ayrıcalıklarıyla standart kullanıcı hesabı ile CentOS 7 çalıştıran sunucu.</span><span class="sxs-lookup"><span data-stu-id="89c31-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="89c31-109">.NET Core çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="89c31-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="89c31-110">Ziyaret [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="89c31-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="89c31-111">En son Önizleme çalışma zamanı altında listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="89c31-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="89c31-112">Seçin ve CentOS/Oracle için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="89c31-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="89c31-113">Mevcut bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="89c31-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="89c31-114">Yayımlama ve uygulamanın üzerine kopyalayın</span><span class="sxs-lookup"><span data-stu-id="89c31-114">Publish and copy over the app</span></span>

<span data-ttu-id="89c31-115">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="89c31-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="89c31-116">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulamayı paketlemek için geliştirme ortamından (örneğin, *bin/yayın/&lt;target_framework_moniker&gt;/ publish*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="89c31-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="89c31-117">Uygulama ayrıca olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) .NET Core çalışma zamanı sunucuda değil sağlamak isterseniz.</span><span class="sxs-lookup"><span data-stu-id="89c31-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="89c31-118">ASP.NET Core uygulaması (örneğin, SCP, SFTP) kuruluşun iş akışınıza tümleştirir bir aracını kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="89c31-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="89c31-119">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="89c31-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="89c31-120">Üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="89c31-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="89c31-121">Bir proxy sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="89c31-121">Configure a proxy server</span></span>

<span data-ttu-id="89c31-122">Ters proxy hizmet dinamik web uygulamaları için ortak bir kurulum var.</span><span class="sxs-lookup"><span data-stu-id="89c31-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="89c31-123">Ters proxy, HTTP isteği sonlandırır ve ASP.NET uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="89c31-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="89c31-124">Bir proxy sunucusu, istemci istekleri istekleri kendisi yerine getirmesini yerine başka bir sunucuya iletir biridir.</span><span class="sxs-lookup"><span data-stu-id="89c31-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="89c31-125">Ters proxy genellikle rastgele istemcileri adına sabit bir hedef iletir.</span><span class="sxs-lookup"><span data-stu-id="89c31-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="89c31-126">Bu kılavuzda, Apache Kestrel ASP.NET Core uygulaması ettiğini aynı sunucu üzerinde çalışan ters proxy olarak yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="89c31-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="89c31-127">Ters proxy tarafından istekleri iletilir çünkü [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer) gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="89c31-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="89c31-128">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` bu yeniden yönlendirme URI'leri ve diğer güvenlik ilkeleri doğru çalışması için başlık.</span><span class="sxs-lookup"><span data-stu-id="89c31-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="89c31-129">Kimlik doğrulaması, bağlantı oluşturma, yeniden yönlendirir ve coğrafi konum, gibi bir düzen bağlı olduğu herhangi bir bileşeni çağrılırken iletilen üstbilgileri Ara sonra yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="89c31-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="89c31-130">Genel kural olarak, tanılama ve hata işleme ara yazılım dışındaki diğer ara yazılımdan önce iletilen üstbilgileri ara yazılım çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="89c31-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="89c31-131">Bu sıralama, iletilen üst bilgi bağlı olan ara yazılım işleme için üstbilgi değerlerini tüketebileceği sağlar.</span><span class="sxs-lookup"><span data-stu-id="89c31-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="89c31-132">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="89c31-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="89c31-133">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="89c31-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="89c31-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="89c31-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="89c31-135">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulaması düzeni ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="89c31-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="89c31-136">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="89c31-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="89c31-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="89c31-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="89c31-138">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer bir kimlik doğrulama düzeni Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="89c31-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="89c31-139">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri:</span><span class="sxs-lookup"><span data-stu-id="89c31-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="89c31-140">Hayır ise [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan başlıkları `None`.</span><span class="sxs-lookup"><span data-stu-id="89c31-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="89c31-141">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="89c31-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="89c31-142">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="89c31-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="89c31-143">Apache yükleyin</span><span class="sxs-lookup"><span data-stu-id="89c31-143">Install Apache</span></span>

<span data-ttu-id="89c31-144">CentOS paketlerinin en son kararlı sürümlerine güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="89c31-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="89c31-145">Apache web sunucusunun tek bir CentOS üzerinde yükleme `yum` komutu:</span><span class="sxs-lookup"><span data-stu-id="89c31-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="89c31-146">Komutu çalıştırdıktan sonra çıktı örneği:</span><span class="sxs-lookup"><span data-stu-id="89c31-146">Sample output after running the command:</span></span>

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
> <span data-ttu-id="89c31-147">CentOS 7 sürümü 64-bit olduğundan bu örnekte, çıktı httpd.86_64 yansıtır.</span><span class="sxs-lookup"><span data-stu-id="89c31-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="89c31-148">Apache yüklendiği doğrulamak için çalıştırın `whereis httpd` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="89c31-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="89c31-149">Apache yapılandırın</span><span class="sxs-lookup"><span data-stu-id="89c31-149">Configure Apache</span></span>

<span data-ttu-id="89c31-150">Yapılandırma dosyaları için Apache içinde bulunduğu `/etc/httpd/conf.d/` dizin.</span><span class="sxs-lookup"><span data-stu-id="89c31-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="89c31-151">Herhangi bir dosya ile *.conf* uzantı modülü yapılandırma dosyalarında yanı sıra alfabetik sırayla işlenir `/etc/httpd/conf.modules.d/`, modülleri yüklemek gerekli dosyaları içeren herhangi bir yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="89c31-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="89c31-152">Adlı bir yapılandırma dosyası oluşturma *hellomvc.conf*, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="89c31-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="89c31-153">`VirtualHost` Blok bir sunucuda bir veya daha fazla dosya içinde birden çok kez görünebilir.</span><span class="sxs-lookup"><span data-stu-id="89c31-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="89c31-154">Önceki yapılandırma dosyasında Apache 80 numaralı bağlantı noktasında ortak trafiği kabul eder.</span><span class="sxs-lookup"><span data-stu-id="89c31-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="89c31-155">Etki alanı `www.example.com` hizmet verilen ve `*.example.com` diğer çözümler için aynı Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="89c31-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="89c31-156">Bkz: [sanal ana bilgisayar adı tabanlı destek](https://httpd.apache.org/docs/current/vhosts/name-based.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="89c31-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="89c31-157">İstekleri kök 127.0.0.1 server örneğinin 5000 numaralı bağlantı noktasına taşınır.</span><span class="sxs-lookup"><span data-stu-id="89c31-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="89c31-158">Çift yönlü iletişim için `ProxyPass` ve `ProxyPassReverse` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="89c31-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="89c31-159">Kestrel'i'nın IP/bağlantı noktasını değiştirmek için bkz [Kestrel: uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="89c31-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="89c31-160">Uygun belirtmek için hata [ServerName yönergesi](https://httpd.apache.org/docs/current/mod/core.html#servername) içinde **VirtualHost** blok uygulamanıza güvenlik açıklarını kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="89c31-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="89c31-161">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanını denetimi bu güvenlik riski yoktur (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="89c31-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="89c31-162">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="89c31-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="89c31-163">Günlüğe kaydetme, başına yapılandırılabilir `VirtualHost` kullanarak `ErrorLog` ve `CustomLog` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="89c31-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="89c31-164">`ErrorLog` Burada sunucu günlüklerine hataları, konum ve `CustomLog` dosya adı ve günlük dosyası biçimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="89c31-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="89c31-165">Bu durumda, burada isteği bilgileri günlüğe budur.</span><span class="sxs-lookup"><span data-stu-id="89c31-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="89c31-166">Her istek için bir satır vardır.</span><span class="sxs-lookup"><span data-stu-id="89c31-166">There's one line for each request.</span></span>

<span data-ttu-id="89c31-167">Dosyayı kaydedin ve test yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="89c31-167">Save the file and test the configuration.</span></span> <span data-ttu-id="89c31-168">Her şeyi geçerse, yanıt olmalıdır `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="89c31-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="89c31-169">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="89c31-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="89c31-170">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="89c31-170">Monitoring the app</span></span>

<span data-ttu-id="89c31-171">Apache, artık yapılan isteklerini iletmek için Kurulum `http://localhost:80` sırasında Kestrel üzerinde çalışan ASP.NET Core uygulaması için `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="89c31-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="89c31-172">Ancak, Apache Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="89c31-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="89c31-173">Kullanım *systemd* başlatmak ve temel alınan web uygulamasını izleme için bir hizmet dosya oluşturun.</span><span class="sxs-lookup"><span data-stu-id="89c31-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="89c31-174">*systemd* başlatılmasını, durdurmasını ve işlemlerini yönetme için çok sayıda güçlü özellikler sağlar init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="89c31-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="89c31-175">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="89c31-175">Create the service file</span></span>

<span data-ttu-id="89c31-176">Hizmet tanım dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="89c31-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="89c31-177">Uygulama için bir örnek hizmeti dosyası:</span><span class="sxs-lookup"><span data-stu-id="89c31-177">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="89c31-178">**Kullanıcı** &mdash; durumunda kullanıcının *apache* kullanılmayan yapılandırmaya göre kullanıcı ilk oluşturulmalı ve dosyaları için uygun sahipliği verilen.</span><span class="sxs-lookup"><span data-stu-id="89c31-178">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="89c31-179">Ortam değişkenlerini okumak yapılandırma sağlayıcıları için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçınılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="89c31-179">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="89c31-180">Yapılandırma dosyasında kullanmak için düzgün bir şekilde atlanan bir değer oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="89c31-180">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="89c31-181">Dosyayı kaydedin ve hizmeti etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="89c31-181">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="89c31-182">Hizmeti başlatın ve çalıştığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="89c31-182">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="89c31-183">İle yönetilen Kestrel ve yapılandırılmış bir ters proxy *systemd*, web uygulaması, tam olarak yapılandırılır ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="89c31-183">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="89c31-184">Yanıt üst bilgilerini inceleyerek **sunucu** üst bilgisi gösterir ASP.NET Core uygulaması Kestrel tarafından sunulur:</span><span class="sxs-lookup"><span data-stu-id="89c31-184">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="89c31-185">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="89c31-185">Viewing logs</span></span>

<span data-ttu-id="89c31-186">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir *systemd*, olaylar ve işlemleri için merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="89c31-186">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="89c31-187">Ancak, bu günlük girişlerini tüm hizmetleri ve işlemleri tarafından yönetilen içerir *systemd*.</span><span class="sxs-lookup"><span data-stu-id="89c31-187">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="89c31-188">Görüntülenecek `kestrel-hellomvc.service`-belirli öğeler, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="89c31-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="89c31-189">Zaman filtresi uygulamak için zaman seçenekleri ile komutu belirtin.</span><span class="sxs-lookup"><span data-stu-id="89c31-189">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="89c31-190">Örneğin, `--since today` geçerli gün için filtre uygulamak veya `--until 1 hour ago` önceki saat girişlerini görmek için.</span><span class="sxs-lookup"><span data-stu-id="89c31-190">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="89c31-191">Daha fazla bilgi için [journalctl man sayfası](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="89c31-191">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="89c31-192">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="89c31-192">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="89c31-193">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="89c31-193">Configure firewall</span></span>

<span data-ttu-id="89c31-194">*Firewalld* ağ bölgeleri için destek Güvenlik Duvarı'nı yönetmek için bir dinamik arka plan programı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="89c31-194">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="89c31-195">Hala bağlantı noktaları ve paket filtreleme iptables tarafından yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="89c31-195">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="89c31-196">*Firewalld* varsayılan olarak yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="89c31-196">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="89c31-197">`yum` paketi yüklemek veya yüklü doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="89c31-197">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="89c31-198">Kullanım `firewalld` yalnızca uygulama için gerekli bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="89c31-198">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="89c31-199">Bu durumda, bağlantı noktası 80 ve 443 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="89c31-199">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="89c31-200">Aşağıdaki komutlar, bağlantı noktaları 80 ve 443'ü açmak için kalıcı olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="89c31-200">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="89c31-201">Güvenlik Duvarı ayarlarını yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="89c31-201">Reload the firewall settings.</span></span> <span data-ttu-id="89c31-202">Kullanılabilir hizmetleri ve bağlantı noktalarını varsayılan bölgede denetleyin.</span><span class="sxs-lookup"><span data-stu-id="89c31-202">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="89c31-203">Seçenekleri kullanılabilir inceleyerek `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="89c31-203">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="89c31-204">SSL yapılandırması</span><span class="sxs-lookup"><span data-stu-id="89c31-204">SSL configuration</span></span>

<span data-ttu-id="89c31-205">SSL için Apache yapılandırmak için *mod_ssl* modülü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="89c31-205">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="89c31-206">Zaman *httpd* modülü yüklendi *mod_ssl* modülü de yüklendi.</span><span class="sxs-lookup"><span data-stu-id="89c31-206">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="89c31-207">Yüklü değildi kullanırsanız `yum` yapılandırmanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="89c31-207">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="89c31-208">SSL zorlama için yükleme `mod_rewrite` etkinleştirme URL yeniden yazma Modülü:</span><span class="sxs-lookup"><span data-stu-id="89c31-208">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="89c31-209">Değiştirme *hellomvc.conf* URL yeniden yazma etkinleştirmek ve bağlantı noktası 443 üzerinden iletişimi güvenli hale getirmek için dosya:</span><span class="sxs-lookup"><span data-stu-id="89c31-209">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="89c31-210">Bu örnek, yerel olarak oluşturulan bir sertifika kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="89c31-210">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="89c31-211">**SSLCertificateFile** etki alanı adının birincil sertifika dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="89c31-211">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="89c31-212">**SSLCertificateKeyFile** CSR oluşturulurken anahtar dosyası oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="89c31-212">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="89c31-213">**SSLCertificateChainFile** ara sertifika dosyasını (varsa) olmalıdır sertifika yetkilisi tarafından sağlandı.</span><span class="sxs-lookup"><span data-stu-id="89c31-213">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="89c31-214">Dosyayı kaydedin ve test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="89c31-214">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="89c31-215">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="89c31-215">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="89c31-216">Ek Apache öneriler</span><span class="sxs-lookup"><span data-stu-id="89c31-216">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="89c31-217">Ek üst bilgiler</span><span class="sxs-lookup"><span data-stu-id="89c31-217">Additional headers</span></span>

<span data-ttu-id="89c31-218">Kötü amaçlı saldırılara karşı güvenli hale getirmek için ya da değiştirildiğinde veya birkaç üstbilgileri vardır.</span><span class="sxs-lookup"><span data-stu-id="89c31-218">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="89c31-219">Emin `mod_headers` modülünün yüklü:</span><span class="sxs-lookup"><span data-stu-id="89c31-219">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="89c31-220">Apache clickjacking saldırılarına karşı güvenli</span><span class="sxs-lookup"><span data-stu-id="89c31-220">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="89c31-221">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)olarak da bilinen bir *UI redress saldırı*, bir kötü amaçlı bir Web sitesi ziyaretçi yere sağladı daha şu anda ziyaret ettiğiniz bir bağlantı veya başka bir sayfaya düğmesine tıklamak saldırıdır.</span><span class="sxs-lookup"><span data-stu-id="89c31-221">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="89c31-222">Kullanım `X-FRAME-OPTIONS` sitesini güvenli hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="89c31-222">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="89c31-223">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="89c31-223">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="89c31-224">Satır Ekle `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="89c31-224">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="89c31-225">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="89c31-225">Save the file.</span></span> <span data-ttu-id="89c31-226">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="89c31-226">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="89c31-227">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="89c31-227">MIME-type sniffing</span></span>

<span data-ttu-id="89c31-228">`X-Content-Type-Options` Üstbilgi engeller Internet Explorer'dan *MIME algılaması* (bir dosyanın belirleme `Content-Type` dosyasının içeriğinden).</span><span class="sxs-lookup"><span data-stu-id="89c31-228">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="89c31-229">Sunucu ayarlarsa `Content-Type` başlığına `text/html` ile `nosniff` seçenek kümesi, Internet Explorer içeriği olarak işler `text/html` dosyanın içeriği ne olursa olsun.</span><span class="sxs-lookup"><span data-stu-id="89c31-229">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="89c31-230">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="89c31-230">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="89c31-231">Satır Ekle `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="89c31-231">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="89c31-232">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="89c31-232">Save the file.</span></span> <span data-ttu-id="89c31-233">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="89c31-233">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="89c31-234">YükDengeleme</span><span class="sxs-lookup"><span data-stu-id="89c31-234">Load Balancing</span></span>

<span data-ttu-id="89c31-235">Bu örnek, Kurulum ve aynı örnek makinede Apache CentOS 7 ve Kestrel yapılandırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="89c31-235">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="89c31-236">Bir tek hata noktası olmaması için; kullanarak *mod_proxy_balancer* ve değiştirme **VirtualHost** Apache proxy sunucunun arkasındaki web uygulamaları birden çok örneğini yönetmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="89c31-236">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="89c31-237">Yapılandırma dosyasında ek bir örneği aşağıda gösterilen `hellomvc` 5001 bağlantı noktası üzerinde çalıştırılacak Kurulum uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="89c31-237">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="89c31-238">*Proxy* bölümü ayarlanmış iki üyeli dengeleyici yapılandırmasına sahip Yük Dengelemesi *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="89c31-238">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="89c31-239">Oran sınırları</span><span class="sxs-lookup"><span data-stu-id="89c31-239">Rate Limits</span></span>

<span data-ttu-id="89c31-240">Kullanarak *mod_ratelimit*, içinde bulunan *httpd* modülü, bant genişliği istemcilerin olabilir sınırlı:</span><span class="sxs-lookup"><span data-stu-id="89c31-240">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="89c31-241">Örnek dosyası kök konumu altında 600 KB/sn olarak bant genişliği sınırları:</span><span class="sxs-lookup"><span data-stu-id="89c31-241">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="89c31-242">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="89c31-242">Additional resources</span></span>

* [<span data-ttu-id="89c31-243">ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="89c31-243">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
