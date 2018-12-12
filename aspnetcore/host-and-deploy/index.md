---
title: Barındırma ve ASP.NET Core dağıtma
author: guardrex
description: Barındırma ortamları hakkında bilgi edinmek ve ASP.NET Core uygulamaları dağıtın.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/index
ms.openlocfilehash: f443a8ee28a859b5075a8bb03016407af9a3ddb1
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284532"
---
# <a name="host-and-deploy-aspnet-core"></a><span data-ttu-id="635e8-103">Barındırma ve ASP.NET Core dağıtma</span><span class="sxs-lookup"><span data-stu-id="635e8-103">Host and deploy ASP.NET Core</span></span>

<span data-ttu-id="635e8-104">Genel olarak bir ASP.NET Core uygulaması için bir barındırma ortamı dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="635e8-104">In general, to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="635e8-105">Barındırma sunucusu üzerindeki bir klasöre yayımlanan uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="635e8-105">Deploy the published app to a folder on the hosting server.</span></span>
* <span data-ttu-id="635e8-106">İstekler geldiğinde ve onu kilitleniyor veya sunucu yeniden başlatıldıktan sonra uygulama yeniden başlatmalarını gerektiğinde, uygulamayı başlatan bir işlem Yöneticisi'ni ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="635e8-106">Set up a process manager that starts the app when requests arrive and restarts the app after it crashes or the server reboots.</span></span>
* <span data-ttu-id="635e8-107">Ters proxy yapılandırma için uygulama isteklerini iletmek için bir ters proxy ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="635e8-107">For configuration of a reverse proxy, set up a reverse proxy to forward requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="635e8-108">Bir klasöre yayımlayın</span><span class="sxs-lookup"><span data-stu-id="635e8-108">Publish to a folder</span></span>

<span data-ttu-id="635e8-109">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut uygulama kodu derler ve uygulamada oturum çalıştırmak için gereken dosyaları kopyalayan bir *yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="635e8-109">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command compiles app code and copies the files required to run the app into a *publish* folder.</span></span> <span data-ttu-id="635e8-110">Visual Studio'dan dağıtırken `dotnet publish` adımından gerçekleştiğinde otomatik olarak önce dosyaları dağıtım hedefe kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="635e8-110">When deploying from Visual Studio, the `dotnet publish` step occurs automatically before the files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="635e8-111">Klasör içeriği</span><span class="sxs-lookup"><span data-stu-id="635e8-111">Folder contents</span></span>

<span data-ttu-id="635e8-112">*Yayımlama* bir veya daha fazla uygulama derleme dosyaları, bağımlılıklar ve isteğe bağlı olarak .NET çalışma zamanı klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="635e8-112">The *publish* folder contains one or more app assembly files, dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="635e8-113">.NET Core uygulaması olarak yayımlanabilir *müstakil dağıtım* veya *framework bağımlı dağıtım*.</span><span class="sxs-lookup"><span data-stu-id="635e8-113">A .NET Core app can be published as *self-contained deployment* or *framework-dependent deployment*.</span></span> <span data-ttu-id="635e8-114">Uygulama kendi içinde ise, .NET çalışma zamanı içeren derleme dosyalarının dahil *yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="635e8-114">If the app is self-contained, the assembly files that contain the .NET runtime are included in the *publish* folder.</span></span> <span data-ttu-id="635e8-115">Uygulama framework bağlı ise, uygulamanın bir sürümü sunucuda yüklü .NET başvuru olduğundan, .NET çalışma zamanı dosyalarını dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="635e8-115">If the app is framework-dependent, the .NET runtime files aren't included because the app has a reference to a version of .NET that's installed on the server.</span></span> <span data-ttu-id="635e8-116">Varsayılan dağıtım modeli framework bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="635e8-116">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="635e8-117">Daha fazla bilgi için [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="635e8-117">For more information, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

<span data-ttu-id="635e8-118">Ek olarak *.exe* ve *.dll* dosyaları *yayımlama* ASP.NET Core uygulaması için klasör genellikle yapılandırma dosyalarını, statik varlıkları ve MVC görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="635e8-118">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span> <span data-ttu-id="635e8-119">Daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="635e8-119">For more information, see <xref:host-and-deploy/directory-structure>.</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="635e8-120">Bir işlem Yöneticisi'ni ayarlayın</span><span class="sxs-lookup"><span data-stu-id="635e8-120">Set up a process manager</span></span>

<span data-ttu-id="635e8-121">ASP.NET Core uygulaması bir sunucu önyüklenir ve onu kilitlenmesi durumunda yeniden başlatılması gerekir bir konsol uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="635e8-121">An ASP.NET Core app is a console app that must be started when a server boots and restarted if it crashes.</span></span> <span data-ttu-id="635e8-122">Başlatır ve yeniden otomatikleştirmek için bir işlem yöneticisi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="635e8-122">To automate starts and restarts, a process manager is required.</span></span> <span data-ttu-id="635e8-123">ASP.NET Core için en yaygın işlem yöneticileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="635e8-123">The most common process managers for ASP.NET Core are:</span></span>

* <span data-ttu-id="635e8-124">Linux</span><span class="sxs-lookup"><span data-stu-id="635e8-124">Linux</span></span>
  * [<span data-ttu-id="635e8-125">Nginx</span><span class="sxs-lookup"><span data-stu-id="635e8-125">Nginx</span></span>](xref:host-and-deploy/linux-nginx)
  * [<span data-ttu-id="635e8-126">Apache</span><span class="sxs-lookup"><span data-stu-id="635e8-126">Apache</span></span>](xref:host-and-deploy/linux-apache)
* <span data-ttu-id="635e8-127">Windows</span><span class="sxs-lookup"><span data-stu-id="635e8-127">Windows</span></span>
  * [<span data-ttu-id="635e8-128">IIS</span><span class="sxs-lookup"><span data-stu-id="635e8-128">IIS</span></span>](xref:host-and-deploy/iis/index)
  * [<span data-ttu-id="635e8-129">Windows hizmeti</span><span class="sxs-lookup"><span data-stu-id="635e8-129">Windows Service</span></span>](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="635e8-130">Bir ters Proxy'yi Ayarlama</span><span class="sxs-lookup"><span data-stu-id="635e8-130">Set up a reverse proxy</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="635e8-131">Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) sunucu [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) bir ters proxy sunucusu olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="635e8-131">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) server, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) can be used as a reverse proxy server.</span></span> <span data-ttu-id="635e8-132">Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir.</span><span class="sxs-lookup"><span data-stu-id="635e8-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

<span data-ttu-id="635e8-133">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="635e8-133">Either configuration&mdash;with or without a reverse proxy server&mdash;is a supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="635e8-134">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="635e8-134">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="635e8-135">Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) sunucu ve Internet'e, kullanım kullanıma [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) ters proxy sunucusu olarak.</span><span class="sxs-lookup"><span data-stu-id="635e8-135">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) server and will be exposed to the Internet, use [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) as a reverse proxy server.</span></span> <span data-ttu-id="635e8-136">Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir.</span><span class="sxs-lookup"><span data-stu-id="635e8-136">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span> <span data-ttu-id="635e8-137">Ters proxy kullanarak ana nedeni, güvenlik sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="635e8-137">The main reason for using a reverse proxy is security.</span></span> <span data-ttu-id="635e8-138">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="635e8-138">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="635e8-139">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="635e8-139">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="635e8-140">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="635e8-140">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="635e8-141">Ek yapılandırma bir uygulama, şema (HTTP/HTTPS) ve uzak IP adresi için erişim isteği geldiği olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="635e8-141">Without additional configuration, an app might not have access to the scheme (HTTP/HTTPS) and the remote IP address where a request originated.</span></span> <span data-ttu-id="635e8-142">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="635e8-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a><span data-ttu-id="635e8-143">Dağıtımları otomatik hale getirmek için Visual Studio ve MSBuild kullanma</span><span class="sxs-lookup"><span data-stu-id="635e8-143">Use Visual Studio and MSBuild to automate deployments</span></span>

<span data-ttu-id="635e8-144">Dağıtım genellikle çıktısı kopyalama yanı sıra ek görevler gerektirir [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) sunucuya.</span><span class="sxs-lookup"><span data-stu-id="635e8-144">Deployment often requires additional tasks besides copying the output from [dotnet publish](/dotnet/core/tools/dotnet-publish) to a server.</span></span> <span data-ttu-id="635e8-145">Örneğin, ek dosyaları gereken veya dışında tutulan *yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="635e8-145">For example, extra files might be required or excluded from the *publish* folder.</span></span> <span data-ttu-id="635e8-146">Visual Studio web dağıtımı için MSBuild kullanır ve MSBuild, dağıtım sırasında birçok diğer görevleri yapmak için özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="635e8-146">Visual Studio uses MSBuild for web deployment, and MSBuild can be customized to do many other tasks during deployment.</span></span> <span data-ttu-id="635e8-147">Daha fazla bilgi için <xref:host-and-deploy/visual-studio-publish-profiles> ve [kullanarak MSBuild ve Team Foundation derlemesi](http://msbuildbook.com/) rehberi.</span><span class="sxs-lookup"><span data-stu-id="635e8-147">For more information, see <xref:host-and-deploy/visual-studio-publish-profiles> and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="635e8-148">Kullanarak [Web'de Yayımla özelliğini](xref:tutorials/publish-to-azure-webapp-using-vs) veya [yerleşik Git desteği](xref:host-and-deploy/azure-apps/azure-continuous-deployment), uygulamaları dağıtılan Azure App Service'te doğrudan Visual Studio'dan.</span><span class="sxs-lookup"><span data-stu-id="635e8-148">By using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or [built-in Git support](xref:host-and-deploy/azure-apps/azure-continuous-deployment), apps can be deployed directly from Visual Studio to the Azure App Service.</span></span> <span data-ttu-id="635e8-149">Azure DevOps hizmetlerini destekleyen [Azure uygulama Hizmeti'ne sürekli dağıtım](/azure/devops/pipelines/targets/webapp).</span><span class="sxs-lookup"><span data-stu-id="635e8-149">Azure DevOps Services supports [continuous deployment to Azure App Service](/azure/devops/pipelines/targets/webapp).</span></span> <span data-ttu-id="635e8-150">Daha fazla bilgi için [ASP.NET Core ve Azure ile DevOps](xref:azure/devops/index).</span><span class="sxs-lookup"><span data-stu-id="635e8-150">For more information, see [DevOps with ASP.NET Core and Azure](xref:azure/devops/index).</span></span>

## <a name="publish-to-azure"></a><span data-ttu-id="635e8-151">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="635e8-151">Publish to Azure</span></span>

<span data-ttu-id="635e8-152">Bkz: <xref:tutorials/publish-to-azure-webapp-using-vs> Visual Studio'yu kullanarak Azure'a uygulama yayımlama konusunda yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="635e8-152">See <xref:tutorials/publish-to-azure-webapp-using-vs> for instructions on how to publish an app to Azure using Visual Studio.</span></span> <span data-ttu-id="635e8-153">Ek bir örnek tarafından sağlanan [Azure'da ASP.NET Core web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="635e8-153">An additional example is provided by [Create an ASP.NET Core web app in Azure](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

## <a name="publish-with-msdeploy-on-windows"></a><span data-ttu-id="635e8-154">Windows üzerinde MSDeploy ile yayımlama</span><span class="sxs-lookup"><span data-stu-id="635e8-154">Publish with MSDeploy on Windows</span></span>

<span data-ttu-id="635e8-155">Bkz: <xref:host-and-deploy/visual-studio-publish-profiles> yayımlama profilini bir Visual Studio ile uygulama yayımlama konusunda yönergeler için kullanarak bir Windows komut istemi dahil olmak üzere [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) komutu.</span><span class="sxs-lookup"><span data-stu-id="635e8-155">See <xref:host-and-deploy/visual-studio-publish-profiles> for instructions on how to publish an app with a Visual Studio publish profile, including from a Windows command prompt using the [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command.</span></span>

## <a name="host-in-a-web-farm"></a><span data-ttu-id="635e8-156">Bir web grubunda barındırın</span><span class="sxs-lookup"><span data-stu-id="635e8-156">Host in a web farm</span></span>

<span data-ttu-id="635e8-157">(Örneğin, uygulamanız ölçeklenebilirlik için birden çok örneğini dağıtımı) web grubu ortamında ASP.NET Core uygulamaları barındırmak için yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/web-farm>.</span><span class="sxs-lookup"><span data-stu-id="635e8-157">For information on configuration for hosting ASP.NET Core apps in a web farm environment (for example, deployment of multiple instances of your app for scalability), see <xref:host-and-deploy/web-farm>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="perform-health-checks"></a><span data-ttu-id="635e8-158">Sistem durumu denetimleri gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="635e8-158">Perform health checks</span></span>

<span data-ttu-id="635e8-159">Sistem durumu denetleme ara yazılım, bir uygulamayı ve bağımlılıklarını sistem durumu denetimleri gerçekleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="635e8-159">Use Health Check Middleware to perform health checks on an app and its dependencies.</span></span> <span data-ttu-id="635e8-160">Daha fazla bilgi için bkz. <xref:host-and-deploy/health-checks>.</span><span class="sxs-lookup"><span data-stu-id="635e8-160">For more information, see <xref:host-and-deploy/health-checks>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="635e8-161">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="635e8-161">Additional resources</span></span>

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
