---
title: Apache ile Linux üzerinde ASP.NET Core barındırma
author: guardrex
description: HTTP trafiğini Kestrel üzerinde çalışan bir ASP.NET Core Web uygulamasına yönlendirmek için CentOS üzerinde bir ters proxy sunucusu olarak Apache 'yi ayarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: shboyer
ms.custom: mvc
ms.date: 12/02/2019
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 730ed1847ec5728657d56db3ccf0f1f5fab6b5dd
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717370"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="88350-103">Apache ile Linux üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="88350-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="88350-104">Sağlayan- [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="88350-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="88350-105">Bu kılavuzu kullanarak, HTTP trafiğinin [Kestrel](xref:fundamentals/servers/kestrel) Server üzerinde çalışan bir ASP.NET Core Web uygulamasına yönlendirilmesini sağlamak Için [CentOS 7](https://www.centos.org/) ' de bir ters proxy sunucusu olarak [Apache](https://httpd.apache.org/) 'yi ayarlamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="88350-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="88350-106">[Mod_proxy uzantısı](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) ve ilgili Modüller sunucunun ters proxy 'sini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="88350-106">The [mod_proxy extension](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88350-107">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="88350-107">Prerequisites</span></span>

* <span data-ttu-id="88350-108">Sudo ayrıcalığına sahip standart bir kullanıcı hesabıyla CentOS 7 çalıştıran sunucu.</span><span class="sxs-lookup"><span data-stu-id="88350-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="88350-109">.NET Core çalışma zamanını sunucuya yükler.</span><span class="sxs-lookup"><span data-stu-id="88350-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="88350-110">[.Net çekirdeğini indir sayfasını](https://dotnet.microsoft.com/download/dotnet-core)ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="88350-110">Visit the [Download .NET Core page](https://dotnet.microsoft.com/download/dotnet-core).</span></span>
   1. <span data-ttu-id="88350-111">En son Önizleme olmayan .NET Core sürümünü seçin.</span><span class="sxs-lookup"><span data-stu-id="88350-111">Select the latest non-preview .NET Core version.</span></span>
   1. <span data-ttu-id="88350-112">**Uygulama çalıştırma-çalışma zamanı**altındaki tabloda en son önizleme dışı çalışma zamanını indirin.</span><span class="sxs-lookup"><span data-stu-id="88350-112">Download the latest non-preview runtime in the table under **Run apps - Runtime**.</span></span>
   1. <span data-ttu-id="88350-113">Linux **Paket Yöneticisi yönergeleri** bağlantısını seçin ve CentOS talimatlarını izleyin.</span><span class="sxs-lookup"><span data-stu-id="88350-113">Select the Linux **Package manager instructions** link and follow the CentOS instructions.</span></span>
* <span data-ttu-id="88350-114">Mevcut bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="88350-114">An existing ASP.NET Core app.</span></span>

<span data-ttu-id="88350-115">Paylaşılan Framework 'ü yükselttikten sonra gelecekte herhangi bir noktada, sunucu tarafından barındırılan ASP.NET Core uygulamaları yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="88350-115">At any point in the future after upgrading the shared framework, restart the ASP.NET Core apps hosted by the server.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="88350-116">Uygulama üzerinde Yayımla ve Kopyala</span><span class="sxs-lookup"><span data-stu-id="88350-116">Publish and copy over the app</span></span>

<span data-ttu-id="88350-117">Uygulamayı [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="88350-117">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="88350-118">Uygulama yerel olarak çalıştırılır ve güvenli bağlantı (HTTPS) yapmak üzere yapılandırılmamışsa aşağıdaki yaklaşımlardan birini benimseyin:</span><span class="sxs-lookup"><span data-stu-id="88350-118">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="88350-119">Uygulamayı güvenli yerel bağlantıları işleyecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="88350-119">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="88350-120">Daha fazla bilgi için [https yapılandırma](#https-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="88350-120">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="88350-121">*Properties/launchSettings. JSON* dosyasındaki `applicationUrl` özelliğinden `https://localhost:5001` (varsa) kaldırın.</span><span class="sxs-lookup"><span data-stu-id="88350-121">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="88350-122">Bir uygulamayı sunucuda çalışabilecek bir dizine (örneğin, *bin/Release/&lt;target_framework_moniker&gt;/Publish*) paketlemek için geliştirme ortamından [DotNet Publish](/dotnet/core/tools/dotnet-publish) çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="88350-122">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="88350-123">Uygulama, sunucuda .NET Core çalışma zamanının bakımını yapmayı tercih ediyorsanız, [kendi kendine içerilen bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) olarak da yayımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="88350-123">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="88350-124">ASP.NET Core uygulamasını, kuruluşun iş akışını (örneğin, SCP, SFTP) tümleştiren bir aracı kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="88350-124">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="88350-125">*Var* dizini altında Web uygulamalarının (örneğin, *var/www/HelloApp*) yerini bulmak yaygındır.</span><span class="sxs-lookup"><span data-stu-id="88350-125">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="88350-126">Bir üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı, uygulamayı yayımlama ve varlıkları sunucuya kopyalama işini yapar.</span><span class="sxs-lookup"><span data-stu-id="88350-126">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="88350-127">Proxy sunucusu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="88350-127">Configure a proxy server</span></span>

<span data-ttu-id="88350-128">Ters proxy, dinamik Web uygulamaları sunmak için ortak bir kurulumtir.</span><span class="sxs-lookup"><span data-stu-id="88350-128">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="88350-129">Ters proxy, HTTP isteğini sonlandırır ve ASP.NET uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="88350-129">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="88350-130">Proxy sunucusu, istemci isteklerini istekleri yerine başka bir sunucuya ileten bir sunucu olur.</span><span class="sxs-lookup"><span data-stu-id="88350-130">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="88350-131">Ters proxy, genellikle rastgele istemciler adına sabit bir hedefe iletilir.</span><span class="sxs-lookup"><span data-stu-id="88350-131">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="88350-132">Bu kılavuzda Apache, Kestrel 'in ASP.NET Core uygulamasına hizmet veren aynı sunucuda çalışan ters proxy olarak yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="88350-132">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="88350-133">İstekler ters proxy tarafından iletileceği için, [Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketindeki [Iletilen üstbilgiler ara yazılımını](xref:host-and-deploy/proxy-load-balancer) kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-133">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="88350-134">Ara yazılım, `X-Forwarded-Proto` üst bilgisini kullanarak `Request.Scheme`güncelleştirir, böylece yeniden yönlendirme URI 'Leri ve diğer güvenlik ilkeleri doğru çalışır.</span><span class="sxs-lookup"><span data-stu-id="88350-134">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="88350-135">Bir şemaya bağlı kimlik doğrulama, bağlantı oluşturma, yeniden yönlendirme ve coğrafi konum gibi herhangi bir bileşen, Iletilen üstbilgiler ara yazılımı çağrıldıktan sonra yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="88350-135">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="88350-136">Genel bir kural olarak, Iletilen üstbilgiler ara yazılımı, tanılama ve hata işleme ara yazılımı dışında diğer ara yazılım ile önce çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="88350-136">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="88350-137">Bu sıralama, iletilen üst bilgi bilgilerine bağlı olan ara yazılımın işleme için üst bilgi değerlerini kullanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="88350-137">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

<span data-ttu-id="88350-138"><xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> veya benzer kimlik doğrulama düzeni ara yazılımını çağırmadan önce `Startup.Configure` <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="88350-138">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="88350-139">`X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerini iletmek için ara yazılımı yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="88350-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="88350-140">Ara yazılım için <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilmemişse, iletmek için varsayılan üstbilgiler `None`.</span><span class="sxs-lookup"><span data-stu-id="88350-140">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="88350-141">Standart localhost adresi (127.0.0.1) dahil olmak üzere geri döngü adreslerinde çalışan proxy 'ler (127.0.0.0/8, [:: 1]), varsayılan olarak güvenilirdir.</span><span class="sxs-lookup"><span data-stu-id="88350-141">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="88350-142">Kuruluş içindeki diğer güvenilir proxy 'ler veya ağlar, Internet ve Web sunucusu arasında istekleri ele alıyorsa, bunları <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> veya <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="88350-142">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="88350-143">Aşağıdaki örnek, alana 10.0.0.100 adresindeki Iletilen üstbilgiler ara sunucusuna `Startup.ConfigureServices``KnownProxies` IP adresinde bir güvenilen ara sunucu ekler:</span><span class="sxs-lookup"><span data-stu-id="88350-143">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="88350-144">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="88350-144">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="88350-145">Apache 'yi yükler</span><span class="sxs-lookup"><span data-stu-id="88350-145">Install Apache</span></span>

<span data-ttu-id="88350-146">CentOS paketlerini en son kararlı sürümlerine güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="88350-146">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="88350-147">Tek bir `yum` komutuyla, CentOS 'a Apache Web sunucusunu yükler:</span><span class="sxs-lookup"><span data-stu-id="88350-147">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="88350-148">Komutu çalıştırdıktan sonra örnek çıkış:</span><span class="sxs-lookup"><span data-stu-id="88350-148">Sample output after running the command:</span></span>

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
> <span data-ttu-id="88350-149">Bu örnekte, CentOS 7 sürümü 64 bit olduğundan çıkış httpd. 86_64 ' i yansıtır.</span><span class="sxs-lookup"><span data-stu-id="88350-149">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="88350-150">Apache 'nin yüklendiğini doğrulamak için, bir komut isteminden `whereis httpd` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="88350-150">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="88350-151">Apache yapılandırma</span><span class="sxs-lookup"><span data-stu-id="88350-151">Configure Apache</span></span>

<span data-ttu-id="88350-152">Apache için yapılandırma dosyaları `/etc/httpd/conf.d/` dizininde bulunur.</span><span class="sxs-lookup"><span data-stu-id="88350-152">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="88350-153">*. Conf* uzantısına sahip herhangi bir dosya, modülleri yüklemek için gereken yapılandırma dosyalarını içeren `/etc/httpd/conf.modules.d/`içindeki modül yapılandırma dosyalarına ek olarak alfabetik sırada işlenir.</span><span class="sxs-lookup"><span data-stu-id="88350-153">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="88350-154">Uygulama için *HelloApp. conf*adlı bir yapılandırma dosyası oluşturun:</span><span class="sxs-lookup"><span data-stu-id="88350-154">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

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

<span data-ttu-id="88350-155">`VirtualHost` bloğu, sunucuda bir veya daha fazla dosyada birden çok kez görünebilir.</span><span class="sxs-lookup"><span data-stu-id="88350-155">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="88350-156">Yukarıdaki yapılandırma dosyasında Apache, 80 numaralı bağlantı noktasında genel trafiği kabul eder.</span><span class="sxs-lookup"><span data-stu-id="88350-156">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="88350-157">Etki alanı `www.example.com` sunulmakta ve `*.example.com` diğer adı aynı Web sitesine çözümleniyor.</span><span class="sxs-lookup"><span data-stu-id="88350-157">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="88350-158">Daha fazla bilgi için bkz. [ad tabanlı sanal konak desteği](https://httpd.apache.org/docs/current/vhosts/name-based.html) .</span><span class="sxs-lookup"><span data-stu-id="88350-158">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="88350-159">İstekler, kökte, sunucunun bağlantı noktası 5000 ' den 127.0.0.1 ' de sunucu üzerinden alınır.</span><span class="sxs-lookup"><span data-stu-id="88350-159">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="88350-160">İki yönlü iletişim için `ProxyPass` ve `ProxyPassReverse` gereklidir.</span><span class="sxs-lookup"><span data-stu-id="88350-160">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="88350-161">Kestrel 'in IP/bağlantı noktasını değiştirmek için bkz. [Kestrel: Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="88350-161">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="88350-162">**VirtualHost** bloğunda uygun bir [ServerName yönergesi](https://httpd.apache.org/docs/current/mod/core.html#servername) belirtmemesi, uygulamanızı güvenlik açıklarına karşı kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="88350-162">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="88350-163">Alt etki alanı joker karakteri bağlama (örneğin, `*.example.com`), tüm üst etki alanını (güvenlik açığı olan `*.com`aksine) kontrol ediyorsanız bu güvenlik riskini ortadan yapmaz.</span><span class="sxs-lookup"><span data-stu-id="88350-163">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="88350-164">Daha fazla bilgi için bkz. [rfc7230 Section-5,4](https://tools.ietf.org/html/rfc7230#section-5.4) .</span><span class="sxs-lookup"><span data-stu-id="88350-164">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="88350-165">Günlüğe kaydetme, `ErrorLog` ve `CustomLog` yönergeleri kullanılarak `VirtualHost` başına yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="88350-165">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="88350-166">`ErrorLog`, sunucunun hataları günlüğe kaydettiği konumdur ve `CustomLog` dosya adını ve günlük dosyasının biçimini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="88350-166">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="88350-167">Bu durumda istek bilgileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="88350-167">In this case, this is where request information is logged.</span></span> <span data-ttu-id="88350-168">Her istek için bir satır vardır.</span><span class="sxs-lookup"><span data-stu-id="88350-168">There's one line for each request.</span></span>

<span data-ttu-id="88350-169">Dosyayı kaydedin ve yapılandırmayı test edin.</span><span class="sxs-lookup"><span data-stu-id="88350-169">Save the file and test the configuration.</span></span> <span data-ttu-id="88350-170">Her şey geçerse yanıt `Syntax [OK]`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="88350-170">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="88350-171">Apache 'i yeniden Başlat:</span><span class="sxs-lookup"><span data-stu-id="88350-171">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="88350-172">Uygulamayı izleme</span><span class="sxs-lookup"><span data-stu-id="88350-172">Monitor the app</span></span>

<span data-ttu-id="88350-173">Apache, artık `http://localhost:80` yapılan istekleri, `http://127.0.0.1:5000`'de Kestrel üzerinde çalışan ASP.NET Core uygulamasına iletmek üzere kuruldu.</span><span class="sxs-lookup"><span data-stu-id="88350-173">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="88350-174">Ancak, Kestrel işlemini yönetmek için Apache ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="88350-174">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="88350-175">Temel Web uygulamasını başlatmak ve izlemek için *systemd* ve hizmet dosyası oluşturma ' yı kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-175">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="88350-176">*systemd* , işlem başlatmak, durdurmak ve yönetmek için birçok güçlü özellik sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="88350-176">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="88350-177">Hizmet dosyasını oluşturma</span><span class="sxs-lookup"><span data-stu-id="88350-177">Create the service file</span></span>

<span data-ttu-id="88350-178">Hizmet tanımı dosyasını oluşturun:</span><span class="sxs-lookup"><span data-stu-id="88350-178">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="88350-179">Uygulama için örnek bir hizmet dosyası:</span><span class="sxs-lookup"><span data-stu-id="88350-179">An example service file for the app:</span></span>

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

<span data-ttu-id="88350-180">Kullanıcı *Apache* yapılandırması tarafından kullanılmıyorsa, önce kullanıcının oluşturulması ve dosyaların uygun sahipliğinin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="88350-180">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="88350-181">Uygulamanın ilk kesme sinyalini aldıktan sonra kapanması için bekleyeceği süreyi yapılandırmak için `TimeoutStopSec` kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-181">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="88350-182">Uygulama bu dönemde kapanmazsa, uygulamayı sonlandırmak için SIGKıLL çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="88350-182">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="88350-183">Değeri unitless saniyeler (örneğin, `150`), zaman aralığı değeri (örneğin, `2min 30s`) veya `infinity` zaman aşımını devre dışı bırakmak için girin.</span><span class="sxs-lookup"><span data-stu-id="88350-183">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="88350-184">`TimeoutStopSec` varsayılan değer olan yönetici yapılandırma dosyasında (*systemd-System. conf*, *System. conf. d*, *systemd-User. conf*, *User. conf. d*) `DefaultTimeoutStopSec` değerini alır.</span><span class="sxs-lookup"><span data-stu-id="88350-184">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="88350-185">Çoğu dağıtım için varsayılan zaman aşımı 90 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="88350-185">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="88350-186">Yapılandırma sağlayıcılarının ortam değişkenlerini okuyabilmesi için bazı değerler (örneğin, SQL bağlantı dizeleri) kaçışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="88350-186">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="88350-187">Yapılandırma dosyasında kullanılmak üzere uygun bir kaçış değeri oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="88350-187">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="88350-188">İki nokta (`:`) ayırıcı, ortam değişkeni adlarında desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="88350-188">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="88350-189">İki nokta üst üste yerine çift alt çizgi (`__`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-189">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="88350-190">Ortam değişkenleri [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider) , ortam değişkenleri yapılandırmaya okurken çift alt çizgileri iki nokta üst üste dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="88350-190">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="88350-191">Aşağıdaki örnekte, bağlantı dizesi anahtarı `ConnectionStrings:DefaultConnection` `ConnectionStrings__DefaultConnection`olarak hizmet tanımı dosyasına ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="88350-191">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="88350-192">Dosyayı kaydedin ve hizmeti etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="88350-192">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="88350-193">Hizmeti başlatın ve çalıştığını doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="88350-193">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="88350-194">Ters proxy yapılandırılmış ve *systemd*üzerinden yönetilen Kestrel, Web uygulaması tam olarak yapılandırılır ve `http://localhost`adresindeki yerel makinedeki bir tarayıcıdan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="88350-194">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="88350-195">Yanıt üst bilgilerini inceleyerek **sunucu** üst bilgisi, ASP.NET Core uygulamasının Kestrel tarafından sunulduğunu belirtir:</span><span class="sxs-lookup"><span data-stu-id="88350-195">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="88350-196">Günlükleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="88350-196">View logs</span></span>

<span data-ttu-id="88350-197">Kestrel kullanan Web uygulaması *systemd*kullanılarak yönetildiğinden, olaylar ve süreçler merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="88350-197">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="88350-198">Ancak, bu günlük *systemd*tarafından yönetilen tüm hizmet ve işlemlere ait girişleri içerir.</span><span class="sxs-lookup"><span data-stu-id="88350-198">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="88350-199">`kestrel-helloapp.service`özgü öğeleri görüntülemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="88350-199">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="88350-200">Zaman filtreleme için komutuyla saat seçeneklerini belirtin.</span><span class="sxs-lookup"><span data-stu-id="88350-200">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="88350-201">Örneğin, geçerli güne filtre uygulamak veya önceki saatin girişlerini görmek için `--until 1 hour ago` `--since today` kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-201">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="88350-202">Daha fazla bilgi için bkz. [journalctl için man sayfası](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="88350-202">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="88350-203">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="88350-203">Data protection</span></span>

<span data-ttu-id="88350-204">[ASP.NET Core veri koruma yığını](xref:security/data-protection/introduction) , kimlik doğrulama ara yazılımı (örneğin, tanımlama bilgisi ara yazılımı) ve siteler arası istek sahteciliğini önleme (CSRF) korumaları dahil olmak üzere birkaç ASP.NET Core [middlewares](xref:fundamentals/middleware/index)tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="88350-204">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="88350-205">Veri koruma API 'Leri Kullanıcı kodu tarafından çağrılmasa bile, veri korumasının kalıcı bir şifreleme [anahtarı deposu](xref:security/data-protection/implementation/key-management)oluşturacak şekilde yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="88350-205">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="88350-206">Veri koruması yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılır.</span><span class="sxs-lookup"><span data-stu-id="88350-206">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="88350-207">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="88350-207">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="88350-208">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçleri geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="88350-208">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="88350-209">Kullanıcıların bir sonraki isteğinde yeniden oturum açması gerekir.</span><span class="sxs-lookup"><span data-stu-id="88350-209">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="88350-210">Anahtar halkası ile korunan tüm veriler artık çözülemez.</span><span class="sxs-lookup"><span data-stu-id="88350-210">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="88350-211">Bu, [CSRF belirteçlerini](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata)içerebilir.</span><span class="sxs-lookup"><span data-stu-id="88350-211">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="88350-212">Veri korumayı, anahtar halkasını sürdürmek ve şifrelemek üzere yapılandırmak için, bkz.:</span><span class="sxs-lookup"><span data-stu-id="88350-212">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="88350-213">Uygulamanın güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="88350-213">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="88350-214">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="88350-214">Configure firewall</span></span>

<span data-ttu-id="88350-215">*Firewallld* , ağ bölgeleri desteğiyle güvenlik duvarını yönetmek için dinamik bir Daemon.</span><span class="sxs-lookup"><span data-stu-id="88350-215">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="88350-216">Bağlantı noktaları ve paket filtrelemesi, Iptables tarafından hala yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="88350-216">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="88350-217">*Firewalld* , varsayılan olarak yüklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="88350-217">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="88350-218">`yum`, paketi yüklemek veya yüklendiğini doğrulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="88350-218">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="88350-219">Yalnızca uygulama için gerekli olan bağlantı noktalarını açmak için `firewalld` kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-219">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="88350-220">Bu durumda, 80 ve 443 numaralı bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="88350-220">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="88350-221">Aşağıdaki komutlar şunları açmak için 80 ve 443 bağlantı noktalarını kalıcı olarak ayarlar:</span><span class="sxs-lookup"><span data-stu-id="88350-221">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="88350-222">Güvenlik Duvarı ayarlarını yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="88350-222">Reload the firewall settings.</span></span> <span data-ttu-id="88350-223">Varsayılan bölgedeki kullanılabilir hizmetleri ve bağlantı noktalarını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="88350-223">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="88350-224">Seçenekler `firewall-cmd -h`inceleyerek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="88350-224">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="https-configuration"></a><span data-ttu-id="88350-225">HTTPS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="88350-225">HTTPS configuration</span></span>

<span data-ttu-id="88350-226">**Uygulamayı güvenli (HTTPS) yerel bağlantılar için yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="88350-226">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="88350-227">[DotNet Run](/dotnet/core/tools/dotnet-run) komutu uygulamanın *Özellikler/launchsettings. JSON* dosyasını kullanır, bu da uygulamayı `applicationUrl` özelliği tarafından belirtilen URL 'lerde dinlemek üzere yapılandırır (örneğin, `https://localhost:5001; http://localhost:5000`).</span><span class="sxs-lookup"><span data-stu-id="88350-227">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="88350-228">Aşağıdaki yaklaşımlardan birini kullanarak uygulamayı `dotnet run` komutu veya geliştirme ortamı için geliştirme sırasında (F5 veya CTRL + Visual Studio Code F5) bir sertifikayı kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="88350-228">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="88350-229">[Varsayılan sertifikayı yapılandırmadan Değiştir](xref:fundamentals/servers/kestrel#configuration) (*önerilir*)</span><span class="sxs-lookup"><span data-stu-id="88350-229">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="88350-230">KestrelServerOptions. ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="88350-230">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="88350-231">**Güvenli (HTTPS) istemci bağlantıları için ters proxy 'yi yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="88350-231">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

<span data-ttu-id="88350-232">HTTPS için Apache 'yi yapılandırmak için *mod_ssl* modülü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="88350-232">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="88350-233">*Httpd* modülü yüklendiğinde *mod_ssl* modülü de yüklendi.</span><span class="sxs-lookup"><span data-stu-id="88350-233">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="88350-234">Yüklenmemişse, yapılandırmaya eklemek için `yum` kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-234">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="88350-235">HTTPS 'yi zorlamak için `mod_rewrite` modülünü yükleyerek URL yeniden yazmayı etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="88350-235">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="88350-236">Bağlantı noktası 443 üzerinde URL yeniden yazma ve güvenli iletişim sağlamak için *HelloApp. conf* dosyasını değiştirin:</span><span class="sxs-lookup"><span data-stu-id="88350-236">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="88350-237">Bu örnek, yerel olarak üretilmiş bir sertifika kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="88350-237">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="88350-238">**Sslcertificatefile** , etki alanı adı için birincil sertifika dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="88350-238">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="88350-239">**Sslcertificatekeyfile** , CSR oluşturulduğunda oluşturulan anahtar dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="88350-239">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="88350-240">**Sslcertificatechainfile** , sertifika yetkilisi tarafından sağlanan ara sertifika dosyası (varsa) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="88350-240">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="88350-241">Dosyayı kaydedin ve yapılandırmayı test edin:</span><span class="sxs-lookup"><span data-stu-id="88350-241">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="88350-242">Apache 'i yeniden Başlat:</span><span class="sxs-lookup"><span data-stu-id="88350-242">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="88350-243">Ek Apache önerileri</span><span class="sxs-lookup"><span data-stu-id="88350-243">Additional Apache suggestions</span></span>

### <a name="restart-apps-with-shared-framework-updates"></a><span data-ttu-id="88350-244">Paylaşılan çerçeve güncelleştirmeleriyle uygulamaları yeniden başlatma</span><span class="sxs-lookup"><span data-stu-id="88350-244">Restart apps with shared framework updates</span></span>

<span data-ttu-id="88350-245">Sunucuda paylaşılan Framework 'ü yükselttikten sonra, sunucu tarafından barındırılan ASP.NET Core uygulamaları yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="88350-245">After upgrading the shared framework on the server, restart the ASP.NET Core apps hosted by the server.</span></span>

### <a name="additional-headers"></a><span data-ttu-id="88350-246">Ek üstbilgiler</span><span class="sxs-lookup"><span data-stu-id="88350-246">Additional headers</span></span>

<span data-ttu-id="88350-247">Kötü amaçlı saldırılara karşı korumak için, değiştirilmesi veya eklenmesi gereken birkaç üstbilgi vardır.</span><span class="sxs-lookup"><span data-stu-id="88350-247">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="88350-248">`mod_headers` modülünün yüklü olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="88350-248">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="88350-249">Tıklama ve tıklama saldırılarına karşı güvenli Apache</span><span class="sxs-lookup"><span data-stu-id="88350-249">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="88350-250">*UI redki saldırısı*olarak da bilinen [tıklama](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), bir Web sitesi ziyaretçisinin bir bağlantı veya düğmeye Şu anda ziyaret ettiğinden farklı bir sayfada tıklanması zor olan kötü amaçlı bir saldırıya neden olur.</span><span class="sxs-lookup"><span data-stu-id="88350-250">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="88350-251">Siteyi güvenli hale getirmek için `X-FRAME-OPTIONS` kullanın.</span><span class="sxs-lookup"><span data-stu-id="88350-251">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="88350-252">Tıklama saldırılarını azaltmak için:</span><span class="sxs-lookup"><span data-stu-id="88350-252">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="88350-253">*Httpd. conf* dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="88350-253">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="88350-254">Satırı `Header append X-FRAME-OPTIONS "SAMEORIGIN"`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="88350-254">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="88350-255">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="88350-255">Save the file.</span></span>
1. <span data-ttu-id="88350-256">Apache 'i yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="88350-256">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="88350-257">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="88350-257">MIME-type sniffing</span></span>

<span data-ttu-id="88350-258">`X-Content-Type-Options` üstbilgisi, Internet Explorer 'ın *MIME algılaması* (dosyanın içeriğinden bir dosyanın `Content-Type` belirleme) engeller.</span><span class="sxs-lookup"><span data-stu-id="88350-258">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="88350-259">Sunucu `Content-Type` üst bilgisini `nosniff` seçenek kümesiyle `text/html` olarak ayarladıysanız, Internet Explorer içeriği dosyanın içeriğinden bağımsız olarak `text/html` olarak işler.</span><span class="sxs-lookup"><span data-stu-id="88350-259">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="88350-260">*Httpd. conf* dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="88350-260">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="88350-261">Satırı `Header set X-Content-Type-Options "nosniff"`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="88350-261">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="88350-262">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="88350-262">Save the file.</span></span> <span data-ttu-id="88350-263">Apache 'i yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="88350-263">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="88350-264">Yük Dengelemesi</span><span class="sxs-lookup"><span data-stu-id="88350-264">Load Balancing</span></span>

<span data-ttu-id="88350-265">Bu örnek, CentOS 7 ve Kestrel üzerinde Apache 'in aynı örnek makinede nasıl ayarlanacağını ve yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="88350-265">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="88350-266">Tek bir hata noktası olmaması için; *mod_proxy_balancer* kullanmak ve **VirtualHost** 'u değiştirmek, Apache proxy sunucusunun arkasındaki Web uygulamalarının birden çok örneğini yönetmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="88350-266">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="88350-267">Aşağıda gösterilen yapılandırma dosyasında, `helloapp` ek bir örneği 5001 numaralı bağlantı noktasında çalışacak şekilde ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="88350-267">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="88350-268">*Proxy* bölümü, Yük Dengeleme *istekleri*için iki üyeli bir dengeleyici yapılandırması ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="88350-268">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="88350-269">Oran limitleri</span><span class="sxs-lookup"><span data-stu-id="88350-269">Rate Limits</span></span>

<span data-ttu-id="88350-270">*Httpd* modülüne eklenen *mod_ratelimit*kullanarak, istemcilerin bant genişliği sınırlandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="88350-270">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="88350-271">Örnek dosya, bant genişliğini kök konumu altında 600 KB/sn olarak sınırlandırır:</span><span class="sxs-lookup"><span data-stu-id="88350-271">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="88350-272">Uzun istek üst bilgisi alanları</span><span class="sxs-lookup"><span data-stu-id="88350-272">Long request header fields</span></span>

<span data-ttu-id="88350-273">Proxy sunucusu varsayılan ayarları, istek üst bilgisi alanlarını genellikle 8.190 bayt ile sınırlar.</span><span class="sxs-lookup"><span data-stu-id="88350-273">Proxy server default settings typically limit request header fields to 8,190 bytes.</span></span> <span data-ttu-id="88350-274">Bir uygulama, varsayılan değerden daha uzun bir süre gerektirebilir (örneğin, [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)kullanan uygulamalar).</span><span class="sxs-lookup"><span data-stu-id="88350-274">An app may require fields longer than the default (for example, apps that use [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span></span> <span data-ttu-id="88350-275">Daha uzun alanlar gerekliyse, proxy sunucusunun [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) yönergesi ayarlamayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="88350-275">If longer fields are required, the proxy server's [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive requires adjustment.</span></span> <span data-ttu-id="88350-276">Uygulanacak değer senaryoya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="88350-276">The value to apply depends on the scenario.</span></span> <span data-ttu-id="88350-277">Daha fazla bilgi için sunucunuzun belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="88350-277">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="88350-278">Gerekli olmadığı takdirde `LimitRequestFieldSize` varsayılan değerini artırmaz.</span><span class="sxs-lookup"><span data-stu-id="88350-278">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="88350-279">Değerin artırılması, kötü amaçlı kullanıcılar tarafından arabellek taşması (taşma) ve hizmet reddi (DoS) saldırıları riskini artırır.</span><span class="sxs-lookup"><span data-stu-id="88350-279">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="88350-280">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="88350-280">Additional resources</span></span>

* [<span data-ttu-id="88350-281">Linux üzerinde .NET Core önkoşulları</span><span class="sxs-lookup"><span data-stu-id="88350-281">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
