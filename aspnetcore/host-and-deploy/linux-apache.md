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
ms.openlocfilehash: b073f00469ada915244a2db71540fd7c971d55ea
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="d13a5-103">ASP.NET Core Apache ile Linux ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="d13a5-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="d13a5-104">Tarafından [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="d13a5-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="d13a5-105">Bu kılavuz kullanılarak nasıl ayarlanacağını öğrenin [Apache](https://httpd.apache.org/) ters proxy sunucusu olarak [CentOS 7](https://www.centos.org/) üzerinde çalışan bir ASP.NET Core web uygulaması için HTTP trafiği yönlendirmek için [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d13a5-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d13a5-106">[Mod_proxy uzantısı](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) ve ilgili modüller sunucunun ters proxy oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d13a5-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d13a5-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="d13a5-107">Prerequisites</span></span>

1. <span data-ttu-id="d13a5-108">Sudo ayrıcalığa sahip standart kullanıcı hesabı ile CentOS 7 çalıştıran sunucu.</span><span class="sxs-lookup"><span data-stu-id="d13a5-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="d13a5-109">.NET çekirdeği çalışma zamanı sunucuya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="d13a5-110">Ziyaret [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="d13a5-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="d13a5-111">En son Önizleme olmayan çalışma zamanı altındaki listeden seçin **çalışma zamanı**.</span><span class="sxs-lookup"><span data-stu-id="d13a5-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="d13a5-112">Seçin ve CentOS/Oracle için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="d13a5-113">Mevcut bir ASP.NET Core uygulama.</span><span class="sxs-lookup"><span data-stu-id="d13a5-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="d13a5-114">Yayımlama ve uygulama kopyalayın</span><span class="sxs-lookup"><span data-stu-id="d13a5-114">Publish and copy over the app</span></span>

<span data-ttu-id="d13a5-115">Uygulama için yapılandırma bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="d13a5-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="d13a5-116">Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir dizine bir uygulama paketi için geliştirme ortamı'ndan (örneğin, *bin/sürüm/&lt;target_framework_moniker&gt;/ yayımlama*), olabilir sunucusunda çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d13a5-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="d13a5-117">Uygulama aynı zamanda olarak yayımlanabilir bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) sunucuda .NET çekirdeği çalışma zamanı sürekli olmayan tercih ederseniz.</span><span class="sxs-lookup"><span data-stu-id="d13a5-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="d13a5-118">ASP.NET Core uygulama (örneğin, SCP, SFTP) kuruluşunuzun akışına tümleşen bir aracı kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="d13a5-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="d13a5-119">Altında web uygulamaları bulmak için ortak olan *var* dizin (örneğin, *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="d13a5-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="d13a5-120">Bir üretim dağıtım senaryosunda sürekli tümleştirme iş akışı uygulama yayımlama ve varlıkları sunucuya kopyalama işlemlerini yapar.</span><span class="sxs-lookup"><span data-stu-id="d13a5-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="d13a5-121">Bir proxy sunucusunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="d13a5-121">Configure a proxy server</span></span>

<span data-ttu-id="d13a5-122">Ters proxy hizmet veren dinamik web uygulamaları için ortak bir kurulur.</span><span class="sxs-lookup"><span data-stu-id="d13a5-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="d13a5-123">Ters proxy HTTP isteği sonlandırır ve ASP.NET uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="d13a5-124">Bir proxy sunucusu, istemci isteklerini istekleri kendisi yerine getirmesini yerine başka bir sunucuya iletir biridir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="d13a5-125">Ters proxy genellikle rastgele istemcileri adına sabit bir hedef iletir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="d13a5-126">Bu kılavuzda, Apache Kestrel ASP.NET Core uygulama hizmet ettiğini aynı sunucu üzerinde çalışan ters proxy yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="d13a5-127">İstekleri tarafından ters proxy iletilir çünkü iletilen üstbilgileri Ara kullanmak [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paket.</span><span class="sxs-lookup"><span data-stu-id="d13a5-127">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="d13a5-128">Ara yazılım güncelleştirmeleri `Request.Scheme`kullanarak `X-Forwarded-Proto` , yeniden yönlendirme URI'ler ve diğer güvenlik ilkelerini doğru çalışması için üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="d13a5-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="d13a5-129">Kimlik doğrulaması ara yazılımı herhangi bir türde kullanırken, iletilen üstbilgileri Ara ilk çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-129">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="d13a5-130">Bu sıralama, kimlik doğrulaması ara yazılımı üstbilgi değerlerini kullanabilir ve doğru yeniden yönlendirme URI oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d13a5-130">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="d13a5-131">Her iki yapılandırma&mdash;ile veya bir ters Ara sunucu olmadan&mdash;geçerli ve desteklenen bir barındırma yapılandırması ASP.NET Core 2.0 veya sonraki uygulamalar içindir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-131">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="d13a5-132">Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d13a5-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d13a5-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d13a5-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d13a5-134">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) veya benzer kimlik doğrulama düzeni ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="d13a5-134">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="d13a5-135">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="d13a5-135">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d13a5-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d13a5-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d13a5-137">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` çağırmadan önce [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) ve [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) veya benzer kimlik doğrulama şeması Ara yazılım.</span><span class="sxs-lookup"><span data-stu-id="d13a5-137">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="d13a5-138">İletmek için ara yazılımını yapılandırma `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgileri:</span><span class="sxs-lookup"><span data-stu-id="d13a5-138">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="d13a5-139">Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen ara yazılımıyla iletmek için varsayılan üstbilgiler `None`.</span><span class="sxs-lookup"><span data-stu-id="d13a5-139">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="d13a5-140">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-140">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="d13a5-141">Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="d13a5-141">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="d13a5-142">Apache yükleyin</span><span class="sxs-lookup"><span data-stu-id="d13a5-142">Install Apache</span></span>

<span data-ttu-id="d13a5-143">CentOS paketleri en son kararlı sürümlerine güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="d13a5-143">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="d13a5-144">Apache web sunucusu üzerinde CentOS tek bir yükleme `yum` komutu:</span><span class="sxs-lookup"><span data-stu-id="d13a5-144">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="d13a5-145">Komutu çalıştırdıktan sonra çıktı örneği:</span><span class="sxs-lookup"><span data-stu-id="d13a5-145">Sample output after running the command:</span></span>

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
> <span data-ttu-id="d13a5-146">Bu örnekte, CentOS 7 sürümü 64-bit olduğundan çıktı httpd.86_64 yansıtır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-146">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="d13a5-147">Apache yüklendiği doğrulamak için çalıştırın `whereis httpd` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="d13a5-147">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="d13a5-148">Apache için ters proxy ayarlarını yapılandır</span><span class="sxs-lookup"><span data-stu-id="d13a5-148">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="d13a5-149">İçinde Apache için yapılandırma dosyalarının bulunduğu `/etc/httpd/conf.d/` dizin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-149">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="d13a5-150">Herhangi dosya ile *.conf* uzantısı modül yapılandırma dosyalarında yanı sıra alfabetik sırada işlenir `/etc/httpd/conf.modules.d/`, herhangi bir yapılandırma içeren modüllerini yüklemek gerekli dosyaları.</span><span class="sxs-lookup"><span data-stu-id="d13a5-150">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="d13a5-151">Adlı bir yapılandırma dosyası oluşturma *hellomvc.conf*, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="d13a5-151">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="d13a5-152">`VirtualHost` Blok, bir sunucu üzerindeki bir veya daha fazla dosyalarda birden çok kez görüntülenebilir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-152">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="d13a5-153">Önceki yapılandırma dosyasında bağlantı noktası 80 üzerinde ortak trafiğin Apache kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d13a5-153">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="d13a5-154">Etki alanı `www.example.com` sunulmasını ve `*.example.com` diğer ad aynı Web sitesine giderir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-154">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="d13a5-155">Bkz: [sanal ana bilgisayar adı tabanlı destek](https://httpd.apache.org/docs/current/vhosts/name-based.html) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="d13a5-155">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="d13a5-156">İstekleri kökündeki 127.0.0.1 server örneğinin 5000 numaralı bağlantı noktasına taşınır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-156">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="d13a5-157">Çift yönlü iletişimi için `ProxyPass` ve `ProxyPassReverse` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-157">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="d13a5-158">Uygun belirtmek için hata [ServerName yönergesi](https://httpd.apache.org/docs/current/mod/core.html#servername) içinde **VirtualHost** blok güvenlik açıkları, uygulamanızın kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="d13a5-158">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="d13a5-159">Alt etki alanı joker bağlama (örneğin, `*.example.com`) tüm üst etki alanı denetlemek, bu güvenlik riski değil (tersine `*.com`, açık olduğu).</span><span class="sxs-lookup"><span data-stu-id="d13a5-159">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d13a5-160">Bkz: [rfc7230 bölüm-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="d13a5-160">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="d13a5-161">Günlüğe kaydetme, başına yapılandırılabilir `VirtualHost` kullanarak `ErrorLog` ve `CustomLog` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="d13a5-161">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="d13a5-162">`ErrorLog` Burada sunucusu günlüklerini hataları, konumu ve `CustomLog` filename ve günlük dosyası biçimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d13a5-162">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="d13a5-163">Bu durumda, istek bilgilerini günlüğe nerede budur.</span><span class="sxs-lookup"><span data-stu-id="d13a5-163">In this case, this is where request information is logged.</span></span> <span data-ttu-id="d13a5-164">Her istek için bir satır vardır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-164">There's one line for each request.</span></span>

<span data-ttu-id="d13a5-165">Dosyayı kaydedin ve yapılandırmayı test etme.</span><span class="sxs-lookup"><span data-stu-id="d13a5-165">Save the file and test the configuration.</span></span> <span data-ttu-id="d13a5-166">Her şeyi geçerse, yanıt olmalıdır `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="d13a5-166">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="d13a5-167">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="d13a5-167">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="d13a5-168">Uygulama izleme</span><span class="sxs-lookup"><span data-stu-id="d13a5-168">Monitoring the app</span></span>

<span data-ttu-id="d13a5-169">Apache olan şimdi yapılan isteklerini iletmek için kurulumu `http://localhost:80` Kestrel çalışan ASP.NET Core App `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="d13a5-169">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="d13a5-170">Ancak, Apache Kestrel işlemini yönetmek için ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="d13a5-170">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="d13a5-171">Kullanım *systemd* ve Başlat ve temel web uygulaması izlemek için bir hizmet dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d13a5-171">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="d13a5-172">*systemd* , başlatma, durdurma ve işlemlerini yönetme için çok güçlü özellikler sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-172">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="d13a5-173">Hizmet dosyası oluşturma</span><span class="sxs-lookup"><span data-stu-id="d13a5-173">Create the service file</span></span>

<span data-ttu-id="d13a5-174">Hizmet tanımı dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d13a5-174">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="d13a5-175">Uygulama için bir örnek hizmet dosyası:</span><span class="sxs-lookup"><span data-stu-id="d13a5-175">An example service file for the app:</span></span>

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
> <span data-ttu-id="d13a5-176">**Kullanıcı** &mdash; , kullanıcı *apache* kullanılmaz yapılandırmanın, kullanıcı ilk oluşturulmalı ve doğru sahipliği dosyaları için verilen.</span><span class="sxs-lookup"><span data-stu-id="d13a5-176">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="d13a5-177">Ortam değişkenleri okumak yapılandırma sağlayıcısı için bazı değerler (örneğin, SQL bağlantı dizelerini) kaçış uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-177">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="d13a5-178">Yapılandırma dosyasında kullanmak için düzgün bir şekilde Atlanan değeri oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="d13a5-178">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="d13a5-179">Dosyayı kaydedin ve hizmeti etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="d13a5-179">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="d13a5-180">Hizmeti başlatın ve çalıştığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="d13a5-180">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="d13a5-181">Ters proxy yapılandırılmış ve üzerinden yönetilen Kestrel *systemd*, web uygulaması tam olarak yapılandırılmamış ve yerel makinede bir tarayıcıdan erişilebilir `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="d13a5-181">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="d13a5-182">Yanıt Üstbilgileri incelemek **Server** üstbilgi gösterir ASP.NET Core uygulama tarafından Kestrel sunulur:</span><span class="sxs-lookup"><span data-stu-id="d13a5-182">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="d13a5-183">Günlükleri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="d13a5-183">Viewing logs</span></span>

<span data-ttu-id="d13a5-184">Web uygulaması bu yana Kestrel kullanarak kullanılarak yönetilir *systemd*, olaylar ve işlemleri merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-184">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="d13a5-185">Ancak, bu günlük tüm işlemler tarafından yönetilen ve Hizmetleri için girişleri içerir *systemd*.</span><span class="sxs-lookup"><span data-stu-id="d13a5-185">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="d13a5-186">Görüntülemek için `kestrel-hellomvc.service`-belirli öğeleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="d13a5-186">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="d13a5-187">Zaman filtresi uygulamak için komutuyla zaman seçeneklerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-187">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="d13a5-188">Örneğin, `--since today` geçerli gün için filtre uygulamak için veya `--until 1 hour ago` önceki saatlik girişlerini görmek için.</span><span class="sxs-lookup"><span data-stu-id="d13a5-188">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="d13a5-189">Daha fazla bilgi için bkz: [journalctl adam sayfa](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="d13a5-189">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="d13a5-190">Uygulama güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="d13a5-190">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="d13a5-191">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d13a5-191">Configure firewall</span></span>

<span data-ttu-id="d13a5-192">*Firewalld* ağ bölgeleri için destek ile Güvenlik Duvarı'nı yönetmek için bir dinamik arka plan programı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-192">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="d13a5-193">Bağlantı noktaları ve paket filtreleme hala iptables tarafından yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-193">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="d13a5-194">*Firewalld* varsayılan olarak yüklü olması.</span><span class="sxs-lookup"><span data-stu-id="d13a5-194">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="d13a5-195">`yum` paketi yüklemek ya da yüklü doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-195">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="d13a5-196">Kullanım `firewalld` yalnızca uygulama için gerekli bağlantı noktalarını açın.</span><span class="sxs-lookup"><span data-stu-id="d13a5-196">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="d13a5-197">Bu durumda, bağlantı noktası 80 ve 443 kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-197">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="d13a5-198">Aşağıdaki komutlar, 80 ve 443'ü açmak için bağlantı noktalarını kalıcı olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="d13a5-198">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="d13a5-199">Güvenlik Duvarı ayarlarını yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-199">Reload the firewall settings.</span></span> <span data-ttu-id="d13a5-200">Kullanılabilir hizmetleri ve bağlantı noktalarını varsayılan bölgede denetleyin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-200">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="d13a5-201">Seçenekleri kullanılabilir inceleyerek `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="d13a5-201">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="d13a5-202">SSL yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d13a5-202">SSL configuration</span></span>

<span data-ttu-id="d13a5-203">SSL, Apache yapılandırmak için *mod_ssl* modülü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-203">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="d13a5-204">Zaman *httpd* modülü yüklendi, *mod_ssl* modülü de yüklendi.</span><span class="sxs-lookup"><span data-stu-id="d13a5-204">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="d13a5-205">Yüklü değildi kullanırsanız `yum` yapılandırmasına eklemek için.</span><span class="sxs-lookup"><span data-stu-id="d13a5-205">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="d13a5-206">SSL zorlamak için yükleme `mod_rewrite` modülü URL yeniden yazma işlemi etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="d13a5-206">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="d13a5-207">Değiştirme *hellomvc.conf* dosya URL yeniden yazma işlemi etkinleştirmek ve bağlantı noktası 443 üzerinden iletişimi güvenli hale getirmek için:</span><span class="sxs-lookup"><span data-stu-id="d13a5-207">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="d13a5-208">Bu örnek, yerel olarak oluşturulan bir sertifika kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="d13a5-208">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="d13a5-209">**SSLCertificateFile** birincil sertifika dosyası için etki alanı adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-209">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="d13a5-210">**SSLCertificateKeyFile** CSR oluşturulduğunda anahtar dosyası oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-210">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="d13a5-211">**SSLCertificateChainFile** ara sertifika dosyası (varsa) olmalıdır sertifika yetkilisi tarafından sağlandı.</span><span class="sxs-lookup"><span data-stu-id="d13a5-211">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="d13a5-212">Dosyayı kaydedin ve test yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="d13a5-212">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="d13a5-213">Apache yeniden başlatın:</span><span class="sxs-lookup"><span data-stu-id="d13a5-213">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="d13a5-214">Ek Apache öneriler</span><span class="sxs-lookup"><span data-stu-id="d13a5-214">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="d13a5-215">Ek üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="d13a5-215">Additional headers</span></span>

<span data-ttu-id="d13a5-216">Kötü amaçlı saldırılara karşı güvenli hale getirmek için ya da değiştirildiğinde veya eklendiğinde birkaç üstbilgileri vardır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-216">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="d13a5-217">Emin `mod_headers` modülü yüklenir:</span><span class="sxs-lookup"><span data-stu-id="d13a5-217">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="d13a5-218">Clickjacking öğesini saldırılarına karşı güvenli Apache</span><span class="sxs-lookup"><span data-stu-id="d13a5-218">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="d13a5-219">[Clickjacking öğesini](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), olarak da bilinen bir *UI redress saldırı*, bir Web sitesine ziyaretçiyi yere sağladı şu anda ziyaret ettiğiniz daha bir bağlantı veya başka bir sayfaya düğmesine tıklamak amaçlı bir saldırı aracıdır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-219">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="d13a5-220">Kullanım `X-FRAME-OPTIONS` sitesi güvenliğini sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="d13a5-220">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="d13a5-221">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d13a5-221">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="d13a5-222">Satırı ekleyin `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="d13a5-222">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="d13a5-223">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-223">Save the file.</span></span> <span data-ttu-id="d13a5-224">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="d13a5-224">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="d13a5-225">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="d13a5-225">MIME-type sniffing</span></span>

<span data-ttu-id="d13a5-226">`X-Content-Type-Options` Üstbilgi engeller Internet Explorer'dan *MIME algılaması* (bir dosyanın belirleme `Content-Type` dosyanın içerikten).</span><span class="sxs-lookup"><span data-stu-id="d13a5-226">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="d13a5-227">Sunucu ayarlarsa `Content-Type` başlığına `text/html` ile `nosniff` seçenek kümesi, Internet Explorer işler içeriği olarak `text/html` dosyanın içeriği ne olursa olsun.</span><span class="sxs-lookup"><span data-stu-id="d13a5-227">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="d13a5-228">Düzen *httpd.conf* dosyası:</span><span class="sxs-lookup"><span data-stu-id="d13a5-228">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="d13a5-229">Satırı ekleyin `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="d13a5-229">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="d13a5-230">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d13a5-230">Save the file.</span></span> <span data-ttu-id="d13a5-231">Apache yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="d13a5-231">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="d13a5-232">YükDengeleme</span><span class="sxs-lookup"><span data-stu-id="d13a5-232">Load Balancing</span></span> 

<span data-ttu-id="d13a5-233">Bu örnekte, Kurulum ve Apache CentOS 7 ve Kestrel aynı örneği makinede yapılandırmak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-233">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="d13a5-234">Tek bir hata noktası olmaması için; kullanarak *mod_proxy_balancer* ve değiştirme **VirtualHost** web uygulamaları Apache proxy sunucunun arkasında birden çok örneğini yönetmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="d13a5-234">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="d13a5-235">Yapılandırma dosyasında ek bir örneği aşağıda gösterilen `hellomvc` 5001 bağlantı noktası üzerinde çalıştırmak için Kurulum uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="d13a5-235">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="d13a5-236">*Proxy* bölüm ayarlanmış iki üyeleriyle dengeleyici yapılandırmasına sahip Yük Dengelemesi *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="d13a5-236">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="d13a5-237">Oran sınırları</span><span class="sxs-lookup"><span data-stu-id="d13a5-237">Rate Limits</span></span>
<span data-ttu-id="d13a5-238">Kullanarak *mod_ratelimit*, içinde bulunan *httpd* modülü, bant genişliği istemcilerin olabilir sınırlı:</span><span class="sxs-lookup"><span data-stu-id="d13a5-238">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="d13a5-239">Örnek dosyası kök konumu altında 600 KB/sn olarak bant genişliği sınırları:</span><span class="sxs-lookup"><span data-stu-id="d13a5-239">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
