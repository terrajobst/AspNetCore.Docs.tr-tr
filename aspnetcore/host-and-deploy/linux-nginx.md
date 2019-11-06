---
title: NGINX ile Linux üzerinde ana bilgisayar ASP.NET Core
author: guardrex
description: HTTP trafiğini Kestrel üzerinde çalışan bir ASP.NET Core Web uygulamasına iletmek için Ubuntu 16,04 üzerinde ters proxy olarak NGINX 'i ayarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: c6ae86ec9ac54ddf2d487fd72156199fbdd029ef
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659871"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="e8d6d-103">NGINX ile Linux üzerinde ana bilgisayar ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8d6d-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="e8d6d-104">, [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="e8d6d-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="e8d6d-105">Bu kılavuzda Ubuntu 16,04 sunucusunda üretime hazır ASP.NET Core ortamı ayarlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="e8d6d-106">Bu yönergeler büyük olasılıkla Ubuntu 'ın daha yeni sürümleriyle çalışır, ancak yönergeler daha yeni sürümlerle sınanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="e8d6d-107">ASP.NET Core tarafından desteklenen diğer Linux dağıtımları hakkında daha fazla bilgi için bkz. [Linux üzerinde .NET Core önkoşulları](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="e8d6d-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="e8d6d-108">Ubuntu 14,04 için, Kestrel işlemini izlemeye yönelik bir çözüm olarak *supervisof* önerilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="e8d6d-109">*systemd* , Ubuntu 14,04 ' de kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="e8d6d-110">Ubuntu 14,04 yönergeleri için [Bu konunun önceki sürümüne](https://github.com/aspnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="e8d6d-111">Bu kılavuz:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-111">This guide:</span></span>

* <span data-ttu-id="e8d6d-112">Mevcut bir ASP.NET Core uygulamasını bir ters proxy sunucusunun arkasına koyar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="e8d6d-113">İstekleri Kestrel Web sunucusuna iletmek için ters proxy sunucusunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="e8d6d-114">Web uygulamasının, bir arka plan programı olarak başlangıcında çalışmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="e8d6d-115">Web uygulamasını yeniden başlatmanıza yardımcı olması için bir işlem yönetim aracı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8d6d-116">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="e8d6d-116">Prerequisites</span></span>

1. <span data-ttu-id="e8d6d-117">Sudo ayrıcalığına sahip standart bir kullanıcı hesabı ile Ubuntu 16,04 sunucusuna erişim.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="e8d6d-118">.NET Core çalışma zamanını sunucuya yükler.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="e8d6d-119">[.NET Core tüm indirmeler sayfasını](https://www.microsoft.com/net/download/all)ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="e8d6d-120">**Çalışma zamanı**altındaki listeden en son önizleme dışı çalışma zamanını seçin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="e8d6d-121">Sunucunun Ubuntu sürümüyle eşleşen Ubuntu yönergelerini seçin ve izleyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="e8d6d-122">Mevcut bir ASP.NET Core uygulaması.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="e8d6d-123">Uygulama üzerinde Yayımla ve Kopyala</span><span class="sxs-lookup"><span data-stu-id="e8d6d-123">Publish and copy over the app</span></span>

<span data-ttu-id="e8d6d-124">Uygulamayı [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="e8d6d-125">Uygulama yerel olarak çalıştırılır ve güvenli bağlantı (HTTPS) yapmak üzere yapılandırılmamışsa aşağıdaki yaklaşımlardan birini benimseyin:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-125">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="e8d6d-126">Uygulamayı güvenli yerel bağlantıları işleyecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-126">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="e8d6d-127">Daha fazla bilgi için [https yapılandırma](#https-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-127">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="e8d6d-128">*Properties/launchSettings. JSON* dosyasındaki `applicationUrl` özelliğinden `https://localhost:5001` (varsa) kaldırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-128">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="e8d6d-129">Bir uygulamayı sunucuda çalışabilecek bir dizine (örneğin, *bin/Release/&lt;target_framework_moniker&gt;/Publish*) paketlemek için geliştirme ortamından [DotNet Publish](/dotnet/core/tools/dotnet-publish) çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-129">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="e8d6d-130">Uygulama, sunucuda .NET Core çalışma zamanının bakımını yapmayı tercih ediyorsanız, [kendi kendine içerilen bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) olarak da yayımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-130">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="e8d6d-131">ASP.NET Core uygulamasını, kuruluşun iş akışını (örneğin, SCP, SFTP) tümleştiren bir aracı kullanarak sunucuya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-131">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="e8d6d-132">*Var* dizini altında Web uygulamalarının (örneğin, *var/www/HelloApp*) yerini bulmak yaygındır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-132">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="e8d6d-133">Bir üretim dağıtım senaryosunda, sürekli tümleştirme iş akışı, uygulamayı yayımlama ve varlıkları sunucuya kopyalama işini yapar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-133">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="e8d6d-134">Uygulamayı test etme:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-134">Test the app:</span></span>

1. <span data-ttu-id="e8d6d-135">Komut satırından uygulamayı çalıştırın: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-135">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="e8d6d-136">Bir tarayıcıda, uygulamanın Linux 'ta yerel olarak çalıştığını doğrulamak için `http://<serveraddress>:<port>` ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-136">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="e8d6d-137">Ters proxy sunucu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e8d6d-137">Configure a reverse proxy server</span></span>

<span data-ttu-id="e8d6d-138">Ters proxy, dinamik Web uygulamaları sunmak için ortak bir kurulumtir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-138">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="e8d6d-139">Ters proxy, HTTP isteğini sonlandırır ve ASP.NET Core uygulamasına iletir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-139">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="e8d6d-140">Ters proxy sunucusu kullanma</span><span class="sxs-lookup"><span data-stu-id="e8d6d-140">Use a reverse proxy server</span></span>

<span data-ttu-id="e8d6d-141">Kestrel, ASP.NET Core ' den dinamik içerik sunmak için harika.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-141">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="e8d6d-142">Ancak, Web 'e sunma özellikleri IIS, Apache veya NGINX gibi sunucu olarak zengin özellik değildir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-142">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="e8d6d-143">Ters bir ara sunucu, statik içerik sunma, istekleri önbelleğe alma, istekleri sıkıştırma ve HTTP sunucusundan HTTPS sonlandırması gibi işleri devreedebilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-143">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.</span></span> <span data-ttu-id="e8d6d-144">Ters ara sunucu, ayrılmış bir makinede bulunabilir veya bir HTTP sunucusu ile birlikte dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-144">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="e8d6d-145">Bu kılavuzun amaçları doğrultusunda, tek bir NGINX örneği kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-145">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="e8d6d-146">Bu, HTTP sunucusu ile birlikte aynı sunucuda çalışır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-146">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="e8d6d-147">Gereksinimlere bağlı olarak, farklı bir kurulum seçilebilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-147">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="e8d6d-148">İstekler ters proxy tarafından iletileceği için, [Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketindeki [Iletilen üstbilgiler ara yazılımını](xref:host-and-deploy/proxy-load-balancer) kullanın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-148">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="e8d6d-149">Ara yazılım, `X-Forwarded-Proto` üst bilgisini kullanarak `Request.Scheme`güncelleştirir, böylece yeniden yönlendirme URI 'Leri ve diğer güvenlik ilkeleri doğru çalışır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-149">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="e8d6d-150">Bir şemaya bağlı kimlik doğrulama, bağlantı oluşturma, yeniden yönlendirme ve coğrafi konum gibi herhangi bir bileşen, Iletilen üstbilgiler ara yazılımı çağrıldıktan sonra yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-150">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="e8d6d-151">Genel bir kural olarak, Iletilen üstbilgiler ara yazılımı, tanılama ve hata işleme ara yazılımı dışında diğer ara yazılım ile önce çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-151">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="e8d6d-152">Bu sıralama, iletilen üst bilgi bilgilerine bağlı olan ara yazılımın işleme için üst bilgi değerlerini kullanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-152">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

<span data-ttu-id="e8d6d-153"><xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> veya benzer kimlik doğrulama düzeni ara yazılımını çağırmadan önce `Startup.Configure` <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-153">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="e8d6d-154">`X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerini iletmek için ara yazılımı yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-154">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="e8d6d-155">Ara yazılım için <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilmemişse, iletmek için varsayılan üstbilgiler `None`.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-155">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="e8d6d-156">Standart localhost adresi (127.0.0.1) dahil olmak üzere geri döngü adreslerinde çalışan proxy 'ler (127.0.0.0/8, [:: 1]), varsayılan olarak güvenilirdir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-156">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="e8d6d-157">Kuruluş içindeki diğer güvenilir proxy 'ler veya ağlar, Internet ve Web sunucusu arasında istekleri ele alıyorsa, bunları <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> veya <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-157">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="e8d6d-158">Aşağıdaki örnek, alana 10.0.0.100 adresindeki Iletilen üstbilgiler ara sunucusuna `Startup.ConfigureServices``KnownProxies` IP adresinde bir güvenilen ara sunucu ekler:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-158">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="e8d6d-159">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-159">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="e8d6d-160">NGINX 'i yükler</span><span class="sxs-lookup"><span data-stu-id="e8d6d-160">Install Nginx</span></span>

<span data-ttu-id="e8d6d-161">NGINX yüklemek için `apt-get` kullanın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-161">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="e8d6d-162">Yükleyici, sistem başlangıcında arka plan programı olarak NGINX çalıştıran bir *systemd* init betiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-162">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="e8d6d-163">[NGINX: resmi deni/Ubuntu paketlerinde](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)Ubuntu yükleme yönergelerini izleyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-163">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="e8d6d-164">İsteğe bağlı NGINX modülleri gerekliyse, kaynaktan NGINX oluşturulması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-164">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="e8d6d-165">NGINX ilk kez yüklendiğinden bu yana şunu çalıştırarak açık olarak başlatın:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-165">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="e8d6d-166">Bir tarayıcının NGINX için varsayılan giriş sayfasını görüntülediğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-166">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="e8d6d-167">Giriş sayfasına `http://<server_IP_address>/index.nginx-debian.html`adresinden ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-167">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="e8d6d-168">NGINX 'i yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e8d6d-168">Configure Nginx</span></span>

<span data-ttu-id="e8d6d-169">İstekleri ASP.NET Core uygulamanıza iletmek için NGINX 'i ters proxy olarak yapılandırmak için */etc/nginx/sites-available/default*değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-169">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="e8d6d-170">Bu dosyayı bir metin düzenleyicisinde açın ve içeriği şu şekilde değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-170">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="e8d6d-171">`server_name` eşleştiğinde NGINX varsayılan sunucuyu kullanır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-171">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="e8d6d-172">Varsayılan sunucu tanımlanmazsa, yapılandırma dosyasındaki ilk sunucu varsayılan sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-172">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="e8d6d-173">En iyi uygulama olarak, yapılandırma dosyanızda 444 durum kodunu döndüren belirli bir varsayılan sunucu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-173">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="e8d6d-174">Varsayılan bir sunucu yapılandırma örneği:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-174">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="e8d6d-175">Önceki yapılandırma dosyası ve varsayılan sunucu ile NGINX, bağlantı noktası 80 üzerinde ana bilgisayar üst bilgisi `example.com` veya `*.example.com`genel trafiği kabul eder.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-175">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="e8d6d-176">Bu konaklarla eşleşmeyen istekler Kestrel 'e iletilemiyor.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-176">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="e8d6d-177">NGINX, eşleşen istekleri `http://localhost:5000`adresindeki Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-177">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="e8d6d-178">Daha fazla bilgi için [NGINX 'in Isteği nasıl işliyorsa öğrenin](https://nginx.org/docs/http/request_processing.html) .</span><span class="sxs-lookup"><span data-stu-id="e8d6d-178">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="e8d6d-179">Kestrel 'in IP/bağlantı noktasını değiştirmek için bkz. [Kestrel: Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="e8d6d-179">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="e8d6d-180">Uygun bir [sunucu_adı yönergesini](https://nginx.org/docs/http/server_names.html) belirtmemesi, uygulamanızı güvenlik açıklarına karşı kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-180">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="e8d6d-181">Alt etki alanı joker karakteri bağlama (örneğin, `*.example.com`), tüm üst etki alanını (güvenlik açığı olan `*.com`aksine) kontrol ediyorsanız bu güvenlik riskini ortadan yapmaz.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-181">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="e8d6d-182">Daha fazla bilgi için bkz. [rfc7230 Section-5,4](https://tools.ietf.org/html/rfc7230#section-5.4) .</span><span class="sxs-lookup"><span data-stu-id="e8d6d-182">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="e8d6d-183">NGINX yapılandırması kurulduktan sonra yapılandırma dosyalarının söz dizimini doğrulamak için `sudo nginx -t` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-183">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="e8d6d-184">Yapılandırma dosyası testi başarılı olursa, `sudo nginx -s reload`çalıştırarak NGINX 'in değişiklikleri seçmesini zorlar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-184">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="e8d6d-185">Uygulamayı sunucuda doğrudan çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-185">To directly run the app on the server:</span></span>

1. <span data-ttu-id="e8d6d-186">Uygulamanın dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-186">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="e8d6d-187">Uygulamayı çalıştırın: `dotnet <app_assembly.dll>`, burada `app_assembly.dll` uygulamanın derleme dosyası adıdır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-187">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="e8d6d-188">Uygulama sunucuda çalışır, ancak Internet üzerinden yanıt vermezse, sunucunun güvenlik duvarını denetleyin ve 80 bağlantı noktasının açık olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="e8d6d-189">Azure Ubuntu VM kullanıyorsanız, gelen bağlantı noktası 80 trafiğine izin veren bir ağ güvenlik grubu (NSG) kuralı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="e8d6d-190">Giden trafik, gelen kuralı etkinleştirildiğinde otomatik olarak verildiği için, giden bağlantı noktası 80 kuralını etkinleştirmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="e8d6d-191">Uygulamayı test etmeyi tamamladıktan sonra komut isteminde `Ctrl+C` ile uygulamayı kapatın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitor-the-app"></a><span data-ttu-id="e8d6d-192">Uygulamayı izleme</span><span class="sxs-lookup"><span data-stu-id="e8d6d-192">Monitor the app</span></span>

<span data-ttu-id="e8d6d-193">Sunucu, `http://<serveraddress>:80` için yapılan istekleri `http://127.0.0.1:5000`adresindeki Kestrel üzerinde çalışan ASP.NET Core uygulamasına iletmek üzere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="e8d6d-194">Ancak, NGINX Kestrel işlemini yönetmek için ayarlanmadı.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="e8d6d-195">*systemd* , temel Web uygulamasını başlatmak ve izlemek üzere bir hizmet dosyası oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="e8d6d-196">*systemd* , işlem başlatmak, durdurmak ve yönetmek için birçok güçlü özellik sağlayan bir init sistemidir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="e8d6d-197">Hizmet dosyasını oluşturma</span><span class="sxs-lookup"><span data-stu-id="e8d6d-197">Create the service file</span></span>

<span data-ttu-id="e8d6d-198">Hizmet tanımı dosyasını oluşturun:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="e8d6d-199">Aşağıda, uygulama için örnek bir hizmet dosyası verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="e8d6d-200">Kullanıcı *www-verileri* yapılandırma tarafından kullanılmıyorsa, burada tanımlanan Kullanıcı önce oluşturulmalı ve dosyalar için uygun sahiplik verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="e8d6d-201">Uygulamanın ilk kesme sinyalini aldıktan sonra kapanması için bekleyeceği süreyi yapılandırmak için `TimeoutStopSec` kullanın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-201">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="e8d6d-202">Uygulama bu dönemde kapanmazsa, uygulamayı sonlandırmak için SIGKıLL çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-202">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="e8d6d-203">Değeri unitless saniyeler (örneğin, `150`), zaman aralığı değeri (örneğin, `2min 30s`) veya `infinity` zaman aşımını devre dışı bırakmak için girin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-203">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="e8d6d-204">`TimeoutStopSec` varsayılan değer olan yönetici yapılandırma dosyasında (*systemd-System. conf*, *System. conf. d*, *systemd-User. conf*, *User. conf. d*) `DefaultTimeoutStopSec` değerini alır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-204">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="e8d6d-205">Çoğu dağıtım için varsayılan zaman aşımı 90 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-205">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="e8d6d-206">Linux, büyük/küçük harfe duyarlı bir dosya sistemine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-206">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="e8d6d-207">ASPNETCORE_ENVIRONMENT 'in "üretim" olarak ayarlanması, yapılandırma dosyası appSettings 'i aramasına neden olur *. Ürün. JSON*, *appSettings. Production. JSON*değil.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-207">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="e8d6d-208">Yapılandırma sağlayıcılarının ortam değişkenlerini okuyabilmesi için bazı değerler (örneğin, SQL bağlantı dizeleri) kaçışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-208">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="e8d6d-209">Yapılandırma dosyasında kullanılmak üzere uygun bir kaçış değeri oluşturmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-209">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="e8d6d-210">İki nokta (`:`) ayırıcı, ortam değişkeni adlarında desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-210">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="e8d6d-211">İki nokta üst üste yerine çift alt çizgi (`__`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-211">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="e8d6d-212">Ortam değişkenleri [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider) , ortam değişkenleri yapılandırmaya okurken çift alt çizgileri iki nokta üst üste dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-212">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="e8d6d-213">Aşağıdaki örnekte, bağlantı dizesi anahtarı `ConnectionStrings:DefaultConnection` `ConnectionStrings__DefaultConnection`olarak hizmet tanımı dosyasına ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-213">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="e8d6d-214">Dosyayı kaydedin ve hizmeti etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-214">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="e8d6d-215">Hizmeti başlatın ve çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-215">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="e8d6d-216">Ters proxy yapılandırılmış ve systemd üzerinden yönetilen Kestrel, Web uygulaması tam olarak yapılandırılır ve `http://localhost`adresindeki yerel makinedeki bir tarayıcıdan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-216">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="e8d6d-217">Ayrıca, uzak bir makineden de erişilebilir, engelleyici olabilecek tüm güvenlik duvarını açabilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-217">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="e8d6d-218">Yanıt üst bilgilerini inceleyerek `Server` üst bilgisi, Kestrel tarafından sunulan ASP.NET Core uygulamasını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-218">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="e8d6d-219">Günlükleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="e8d6d-219">View logs</span></span>

<span data-ttu-id="e8d6d-220">Kestrel kullanan Web uygulaması `systemd`kullanılarak yönetildiğinden, tüm olaylar ve süreçler merkezi bir günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-220">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="e8d6d-221">Ancak, bu günlük `systemd`tarafından yönetilen tüm hizmetler ve süreçler için tüm girişleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-221">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="e8d6d-222">`kestrel-helloapp.service`özgü öğeleri görüntülemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-222">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="e8d6d-223">Daha fazla filtreleme için `--since today`, `--until 1 hour ago` veya bunların bir bileşimi gibi zaman seçenekleri döndürülen girdi miktarını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-223">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="e8d6d-224">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="e8d6d-224">Data protection</span></span>

<span data-ttu-id="e8d6d-225">[ASP.NET Core veri koruma yığını](xref:security/data-protection/introduction) , kimlik doğrulama ara yazılımı (örneğin, tanımlama bilgisi ara yazılımı) ve siteler arası istek sahteciliğini önleme (CSRF) korumaları dahil olmak üzere birkaç ASP.NET Core [middlewares](xref:fundamentals/middleware/index)tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-225">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="e8d6d-226">Veri koruma API 'Leri Kullanıcı kodu tarafından çağrılmasa bile, veri korumasının kalıcı bir şifreleme [anahtarı deposu](xref:security/data-protection/implementation/key-management)oluşturacak şekilde yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-226">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="e8d6d-227">Veri koruması yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-227">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="e8d6d-228">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-228">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="e8d6d-229">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçleri geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-229">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="e8d6d-230">Kullanıcıların bir sonraki isteğinde yeniden oturum açması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-230">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="e8d6d-231">Anahtar halkası ile korunan tüm veriler artık çözülemez.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-231">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="e8d6d-232">Bu, [CSRF belirteçlerini](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata)içerebilir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-232">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="e8d6d-233">Veri korumayı, anahtar halkasını sürdürmek ve şifrelemek üzere yapılandırmak için, bkz.:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-233">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a><span data-ttu-id="e8d6d-234">Uzun istek üst bilgisi alanları</span><span class="sxs-lookup"><span data-stu-id="e8d6d-234">Long request header fields</span></span>

<span data-ttu-id="e8d6d-235">Proxy sunucusu varsayılan ayarları, platforma bağlı olarak genellikle istek üst bilgisi alanlarını 4 K veya 8 K ile sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-235">Proxy server default settings typically limit request header fields to 4 K or 8 K depending on the platform.</span></span> <span data-ttu-id="e8d6d-236">Bir uygulama, varsayılan değerden daha uzun bir süre gerektirebilir (örneğin, [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)kullanan uygulamalar).</span><span class="sxs-lookup"><span data-stu-id="e8d6d-236">An app may require fields longer than the default (for example, apps that use [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span></span> <span data-ttu-id="e8d6d-237">Daha uzun alanlar gerekliyse, proxy sunucusunun varsayılan ayarları ayarlama gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-237">If longer fields are required, the proxy server's default settings require adjustment.</span></span> <span data-ttu-id="e8d6d-238">Uygulanacak değerler senaryoya göre değişir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-238">The values to apply depend on the scenario.</span></span> <span data-ttu-id="e8d6d-239">Daha fazla bilgi için sunucunuzun belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-239">For more information, see your server's documentation.</span></span>

* [<span data-ttu-id="e8d6d-240">proxy_buffer_size</span><span class="sxs-lookup"><span data-stu-id="e8d6d-240">proxy_buffer_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [<span data-ttu-id="e8d6d-241">proxy_buffers</span><span class="sxs-lookup"><span data-stu-id="e8d6d-241">proxy_buffers</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [<span data-ttu-id="e8d6d-242">proxy_busy_buffers_size</span><span class="sxs-lookup"><span data-stu-id="e8d6d-242">proxy_busy_buffers_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [<span data-ttu-id="e8d6d-243">large_client_header_buffers</span><span class="sxs-lookup"><span data-stu-id="e8d6d-243">large_client_header_buffers</span></span>](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> <span data-ttu-id="e8d6d-244">Gerekli olmadığı takdirde proxy arabelleklerinin varsayılan değerlerini artırmaz.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-244">Don't increase the default values of proxy buffers unless necessary.</span></span> <span data-ttu-id="e8d6d-245">Bu değerlerin artırılması, kötü amaçlı kullanıcılar tarafından arabellek taşması (taşma) ve hizmet reddi (DoS) saldırıları riskini artırır.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-245">Increasing these values increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="secure-the-app"></a><span data-ttu-id="e8d6d-246">Uygulamanın güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="e8d6d-246">Secure the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="e8d6d-247">AppArmor etkinleştir</span><span class="sxs-lookup"><span data-stu-id="e8d6d-247">Enable AppArmor</span></span>

<span data-ttu-id="e8d6d-248">Linux güvenlik modülleri (LSM), Linux 2,6 ' den beri Linux çekirdeğinin parçası olan bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-248">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="e8d6d-249">LSM, güvenlik modüllerinin farklı uygulamalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-249">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="e8d6d-250">[AppArmor](https://wiki.ubuntu.com/AppArmor) , programı sınırlı bir kaynak kümesiyle sınırlandırarak zorunlu bir Access Control sistemi uygulayan bir LSM 'dir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-250">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="e8d6d-251">AppArmor etkinleştirildiğinden ve düzgün şekilde yapılandırıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-251">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configure-the-firewall"></a><span data-ttu-id="e8d6d-252">Güvenlik duvarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e8d6d-252">Configure the firewall</span></span>

<span data-ttu-id="e8d6d-253">Kullanımda olmayan tüm dış bağlantı noktalarını kapatın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-253">Close off all external ports that are not in use.</span></span> <span data-ttu-id="e8d6d-254">Karmaşık olmayan güvenlik duvarı (UW), güvenlik duvarını yapılandırmak için bir komut satırı arabirimi sağlayarak `iptables` için bir ön uç sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-254">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="e8d6d-255">Bir güvenlik duvarı, doğru yapılandırılmamışsa tüm sisteme erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-255">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="e8d6d-256">Kendisine bağlanmak için SSH kullanıyorsanız doğru SSH bağlantı noktasını belirtmemesi Sistem oturumunuzu etkin bir şekilde kilitleyecek.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-256">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="e8d6d-257">Varsayılan bağlantı noktası 22 ' dir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-257">The default port is 22.</span></span> <span data-ttu-id="e8d6d-258">Daha fazla bilgi için bkz. [UFW 'ye giriş](https://help.ubuntu.com/community/UFW) ve [el ile](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="e8d6d-258">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="e8d6d-259">`ufw` yükleyip, gereken bağlantı noktalarında trafiğe izin verecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-259">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a><span data-ttu-id="e8d6d-260">Güvenli NGINX</span><span class="sxs-lookup"><span data-stu-id="e8d6d-260">Secure Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="e8d6d-261">NGINX yanıt adını değiştirme</span><span class="sxs-lookup"><span data-stu-id="e8d6d-261">Change the Nginx response name</span></span>

<span data-ttu-id="e8d6d-262">*Src/http/ngx_http_header_filter_module. c*'yi düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-262">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="e8d6d-263">Seçenekleri Yapılandır</span><span class="sxs-lookup"><span data-stu-id="e8d6d-263">Configure options</span></span>

<span data-ttu-id="e8d6d-264">Sunucuyu gerekli olan ek modüllerle yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-264">Configure the server with additional required modules.</span></span> <span data-ttu-id="e8d6d-265">Uygulamayı sağlamlaştırmak için [ModSecurity](https://www.modsecurity.org/)gibi bir Web uygulaması güvenlik duvarı kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-265">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="https-configuration"></a><span data-ttu-id="e8d6d-266">HTTPS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="e8d6d-266">HTTPS configuration</span></span>

<span data-ttu-id="e8d6d-267">**Uygulamayı güvenli (HTTPS) yerel bağlantılar için yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="e8d6d-267">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="e8d6d-268">[DotNet Run](/dotnet/core/tools/dotnet-run) komutu uygulamanın *Özellikler/launchsettings. JSON* dosyasını kullanır, bu da uygulamayı `applicationUrl` özelliği tarafından belirtilen URL 'lerde dinlemek üzere yapılandırır (örneğin, `https://localhost:5001; http://localhost:5000`).</span><span class="sxs-lookup"><span data-stu-id="e8d6d-268">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="e8d6d-269">Aşağıdaki yaklaşımlardan birini kullanarak uygulamayı `dotnet run` komutu veya geliştirme ortamı için geliştirme sırasında (F5 veya CTRL + Visual Studio Code F5) bir sertifikayı kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-269">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="e8d6d-270">[Varsayılan sertifikayı yapılandırmadan Değiştir](xref:fundamentals/servers/kestrel#configuration) (*önerilir*)</span><span class="sxs-lookup"><span data-stu-id="e8d6d-270">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="e8d6d-271">KestrelServerOptions. ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="e8d6d-271">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="e8d6d-272">**Güvenli (HTTPS) istemci bağlantıları için ters proxy 'yi yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="e8d6d-272">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

* <span data-ttu-id="e8d6d-273">Güvenilen bir sertifika yetkilisi (CA) tarafından verilen geçerli bir sertifika belirterek, sunucu `443` bağlantı noktasında HTTPS trafiğini dinleyecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-273">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="e8d6d-274">Aşağıdaki */etc/nginx/nginx.conf* dosyasında gösterilen bazı uygulamalardan yararlanarak güvenliği en iyi şekilde yapın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-274">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="e8d6d-275">Daha güçlü bir şifre seçme ve HTTP üzerinden tüm trafiği HTTPS 'ye yeniden yönlendirme örnekleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-275">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="e8d6d-276">`HTTP Strict-Transport-Security` (HSTS) üstbilgisi eklemek, istemci tarafından yapılan tüm sonraki isteklerin HTTPS üzerinden yapılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-276">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="e8d6d-277">Daha sonra HTTPS 'nin devre dışı bırakılacağı için HSTS üst bilgisini eklemeyin veya uygun bir `max-age` seçmeyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-277">Don't add the HSTS header or chose an appropriate `max-age` if HTTPS will be disabled in the future.</span></span>

<span data-ttu-id="e8d6d-278">*/Etc/nginx/proxy.conf* yapılandırma dosyasını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-278">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="e8d6d-279">*/Etc/nginx/nginx.conf* yapılandırma dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-279">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="e8d6d-280">Örnek, tek bir yapılandırma dosyasında hem `http` hem de `server` bölümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-280">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="e8d6d-281">Tıklama mercekten NGINX 'i güvenli hale getirme</span><span class="sxs-lookup"><span data-stu-id="e8d6d-281">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="e8d6d-282">*UI redki saldırısı*olarak da bilinen [tıklama](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), bir Web sitesi ziyaretçisinin bir bağlantı veya düğmeye Şu anda ziyaret ettiğinden farklı bir sayfada tıklanması zor olan kötü amaçlı bir saldırıya neden olur.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-282">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="e8d6d-283">Siteyi güvenli hale getirmek için `X-FRAME-OPTIONS` kullanın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-283">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="e8d6d-284">Tıklama saldırılarını azaltmak için:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-284">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="e8d6d-285">*NGINX. conf* dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-285">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="e8d6d-286">Satırı `add_header X-Frame-Options "SAMEORIGIN";`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-286">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="e8d6d-287">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-287">Save the file.</span></span>
1. <span data-ttu-id="e8d6d-288">NGINX 'i yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-288">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="e8d6d-289">MIME türü algılaması</span><span class="sxs-lookup"><span data-stu-id="e8d6d-289">MIME-type sniffing</span></span>

<span data-ttu-id="e8d6d-290">Bu üst bilgi, tarayıcının, yanıt içerik türünü geçersiz kılmamasını bildiren, büyük bir olasılıkla, MIME tarafından yapılan bir yanıtın bir yanıt olarak bildirimde bulunmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-290">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="e8d6d-291">`nosniff` seçeneği ile, sunucu içeriği "metin/html" ise, tarayıcı bunu "metin/html" olarak işler.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-291">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="e8d6d-292">*NGINX. conf* dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="e8d6d-292">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="e8d6d-293">Satırı `add_header X-Content-Type-Options "nosniff";` ekleyin ve dosyayı kaydedin, sonra NGINX 'i yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="e8d6d-293">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8d6d-294">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e8d6d-294">Additional resources</span></span>

* [<span data-ttu-id="e8d6d-295">Linux üzerinde .NET Core önkoşulları</span><span class="sxs-lookup"><span data-stu-id="e8d6d-295">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* [<span data-ttu-id="e8d6d-296">NGINX: Ikili yayınlar: resmi olmayan/Ubuntu Paketleri</span><span class="sxs-lookup"><span data-stu-id="e8d6d-296">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="e8d6d-297">NGıNX: Iletilen üstbilgiyi kullanma</span><span class="sxs-lookup"><span data-stu-id="e8d6d-297">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
