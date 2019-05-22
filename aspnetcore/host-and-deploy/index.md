---
title: Barındırma ve ASP.NET Core dağıtma
author: guardrex
description: Barındırma ortamları hakkında bilgi edinmek ve ASP.NET Core uygulamaları dağıtın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/index
ms.openlocfilehash: 068c5f357b8010d78493a78a10c353e3317f3867
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969894"
---
# <a name="host-and-deploy-aspnet-core"></a><span data-ttu-id="63246-103">Barındırma ve ASP.NET Core dağıtma</span><span class="sxs-lookup"><span data-stu-id="63246-103">Host and deploy ASP.NET Core</span></span>

<span data-ttu-id="63246-104">Genel olarak bir ASP.NET Core uygulaması için bir barındırma ortamı dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="63246-104">In general, to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="63246-105">Barındırma sunucusu üzerindeki bir klasöre yayımlanan uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="63246-105">Deploy the published app to a folder on the hosting server.</span></span>
* <span data-ttu-id="63246-106">İstekler geldiğinde ve onu kilitleniyor veya sunucu yeniden başlatıldıktan sonra uygulama yeniden başlatmalarını gerektiğinde, uygulamayı başlatan bir işlem Yöneticisi'ni ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="63246-106">Set up a process manager that starts the app when requests arrive and restarts the app after it crashes or the server reboots.</span></span>
* <span data-ttu-id="63246-107">Ters proxy yapılandırma için uygulama isteklerini iletmek için bir ters proxy ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="63246-107">For configuration of a reverse proxy, set up a reverse proxy to forward requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="63246-108">Bir klasöre yayımlayın</span><span class="sxs-lookup"><span data-stu-id="63246-108">Publish to a folder</span></span>

<span data-ttu-id="63246-109">[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut uygulama kodu derler ve uygulamada oturum çalıştırmak için gereken dosyaları kopyalayan bir *yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="63246-109">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command compiles app code and copies the files required to run the app into a *publish* folder.</span></span> <span data-ttu-id="63246-110">Visual Studio'dan dağıtırken `dotnet publish` adımından gerçekleştiğinde otomatik olarak önce dosyaları dağıtım hedefe kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="63246-110">When deploying from Visual Studio, the `dotnet publish` step occurs automatically before the files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="63246-111">Klasör içeriği</span><span class="sxs-lookup"><span data-stu-id="63246-111">Folder contents</span></span>

<span data-ttu-id="63246-112">*Yayımlama* bir veya daha fazla uygulama derleme dosyaları, bağımlılıklar ve isteğe bağlı olarak .NET çalışma zamanı klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="63246-112">The *publish* folder contains one or more app assembly files, dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="63246-113">.NET Core uygulaması olarak yayımlanabilir *müstakil dağıtım* veya *framework bağımlı dağıtım*.</span><span class="sxs-lookup"><span data-stu-id="63246-113">A .NET Core app can be published as *self-contained deployment* or *framework-dependent deployment*.</span></span> <span data-ttu-id="63246-114">Uygulama kendi içinde ise, .NET çalışma zamanı içeren derleme dosyalarının dahil *yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="63246-114">If the app is self-contained, the assembly files that contain the .NET runtime are included in the *publish* folder.</span></span> <span data-ttu-id="63246-115">Uygulama framework bağlı ise, uygulamanın bir sürümü sunucuda yüklü .NET başvuru olduğundan, .NET çalışma zamanı dosyalarını dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="63246-115">If the app is framework-dependent, the .NET runtime files aren't included because the app has a reference to a version of .NET that's installed on the server.</span></span> <span data-ttu-id="63246-116">Varsayılan dağıtım modeli framework bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="63246-116">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="63246-117">Daha fazla bilgi için [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="63246-117">For more information, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

<span data-ttu-id="63246-118">Ek olarak *.exe* ve *.dll* dosyaları *yayımlama* ASP.NET Core uygulaması için klasör genellikle yapılandırma dosyalarını, statik varlıkları ve MVC görünümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="63246-118">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span> <span data-ttu-id="63246-119">Daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="63246-119">For more information, see <xref:host-and-deploy/directory-structure>.</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="63246-120">Bir işlem Yöneticisi'ni ayarlayın</span><span class="sxs-lookup"><span data-stu-id="63246-120">Set up a process manager</span></span>

<span data-ttu-id="63246-121">ASP.NET Core uygulaması bir sunucu önyüklenir ve onu kilitlenmesi durumunda yeniden başlatılması gerekir bir konsol uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="63246-121">An ASP.NET Core app is a console app that must be started when a server boots and restarted if it crashes.</span></span> <span data-ttu-id="63246-122">Başlatır ve yeniden otomatikleştirmek için bir işlem yöneticisi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="63246-122">To automate starts and restarts, a process manager is required.</span></span> <span data-ttu-id="63246-123">ASP.NET Core için en yaygın işlem yöneticileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="63246-123">The most common process managers for ASP.NET Core are:</span></span>

* <span data-ttu-id="63246-124">Linux</span><span class="sxs-lookup"><span data-stu-id="63246-124">Linux</span></span>
  * [<span data-ttu-id="63246-125">Nginx</span><span class="sxs-lookup"><span data-stu-id="63246-125">Nginx</span></span>](xref:host-and-deploy/linux-nginx)
  * [<span data-ttu-id="63246-126">Apache</span><span class="sxs-lookup"><span data-stu-id="63246-126">Apache</span></span>](xref:host-and-deploy/linux-apache)
* <span data-ttu-id="63246-127">Windows</span><span class="sxs-lookup"><span data-stu-id="63246-127">Windows</span></span>
  * [<span data-ttu-id="63246-128">IIS</span><span class="sxs-lookup"><span data-stu-id="63246-128">IIS</span></span>](xref:host-and-deploy/iis/index)
  * [<span data-ttu-id="63246-129">Windows hizmeti</span><span class="sxs-lookup"><span data-stu-id="63246-129">Windows Service</span></span>](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="63246-130">Bir ters Proxy'yi Ayarlama</span><span class="sxs-lookup"><span data-stu-id="63246-130">Set up a reverse proxy</span></span>

<span data-ttu-id="63246-131">Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) sunucu [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) bir ters proxy sunucusu olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63246-131">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) server, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) can be used as a reverse proxy server.</span></span> <span data-ttu-id="63246-132">Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir.</span><span class="sxs-lookup"><span data-stu-id="63246-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

<span data-ttu-id="63246-133">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;desteklenen bir barındırma yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="63246-133">Either configuration&mdash;with or without a reverse proxy server&mdash;is a supported hosting configuration.</span></span> <span data-ttu-id="63246-134">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="63246-134">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="63246-135">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="63246-135">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="63246-136">Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="63246-136">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="63246-137">Ek yapılandırma bir uygulama, şema (HTTP/HTTPS) ve uzak IP adresi için erişim isteği geldiği olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="63246-137">Without additional configuration, an app might not have access to the scheme (HTTP/HTTPS) and the remote IP address where a request originated.</span></span> <span data-ttu-id="63246-138">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="63246-138">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a><span data-ttu-id="63246-139">Dağıtımları otomatik hale getirmek için Visual Studio ve MSBuild kullanma</span><span class="sxs-lookup"><span data-stu-id="63246-139">Use Visual Studio and MSBuild to automate deployments</span></span>

<span data-ttu-id="63246-140">Dağıtım genellikle çıktısı kopyalama yanı sıra ek görevler gerektirir [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) sunucuya.</span><span class="sxs-lookup"><span data-stu-id="63246-140">Deployment often requires additional tasks besides copying the output from [dotnet publish](/dotnet/core/tools/dotnet-publish) to a server.</span></span> <span data-ttu-id="63246-141">Örneğin, ek dosyaları gereken veya dışında tutulan *yayımlama* klasör.</span><span class="sxs-lookup"><span data-stu-id="63246-141">For example, extra files might be required or excluded from the *publish* folder.</span></span> <span data-ttu-id="63246-142">Visual Studio web dağıtımı için MSBuild kullanır ve MSBuild, dağıtım sırasında birçok diğer görevleri yapmak için özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="63246-142">Visual Studio uses MSBuild for web deployment, and MSBuild can be customized to do many other tasks during deployment.</span></span> <span data-ttu-id="63246-143">Daha fazla bilgi için <xref:host-and-deploy/visual-studio-publish-profiles> ve [kullanarak MSBuild ve Team Foundation derlemesi](http://msbuildbook.com/) rehberi.</span><span class="sxs-lookup"><span data-stu-id="63246-143">For more information, see <xref:host-and-deploy/visual-studio-publish-profiles> and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="63246-144">Kullanarak [Web'de Yayımla özelliğini](xref:tutorials/publish-to-azure-webapp-using-vs) veya [yerleşik Git desteği](xref:host-and-deploy/azure-apps/azure-continuous-deployment), uygulamaları dağıtılan Azure App Service'te doğrudan Visual Studio'dan.</span><span class="sxs-lookup"><span data-stu-id="63246-144">By using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or [built-in Git support](xref:host-and-deploy/azure-apps/azure-continuous-deployment), apps can be deployed directly from Visual Studio to the Azure App Service.</span></span> <span data-ttu-id="63246-145">Azure DevOps hizmetlerini destekleyen [Azure uygulama Hizmeti'ne sürekli dağıtım](/azure/devops/pipelines/targets/webapp).</span><span class="sxs-lookup"><span data-stu-id="63246-145">Azure DevOps Services supports [continuous deployment to Azure App Service](/azure/devops/pipelines/targets/webapp).</span></span> <span data-ttu-id="63246-146">Daha fazla bilgi için [ASP.NET Core ve Azure ile DevOps](xref:azure/devops/index).</span><span class="sxs-lookup"><span data-stu-id="63246-146">For more information, see [DevOps with ASP.NET Core and Azure](xref:azure/devops/index).</span></span>

## <a name="publish-to-azure"></a><span data-ttu-id="63246-147">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="63246-147">Publish to Azure</span></span>

<span data-ttu-id="63246-148">Bkz: <xref:tutorials/publish-to-azure-webapp-using-vs> Visual Studio'yu kullanarak Azure'a uygulama yayımlama konusunda yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="63246-148">See <xref:tutorials/publish-to-azure-webapp-using-vs> for instructions on how to publish an app to Azure using Visual Studio.</span></span> <span data-ttu-id="63246-149">Ek bir örnek tarafından sağlanan [Azure'da ASP.NET Core web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="63246-149">An additional example is provided by [Create an ASP.NET Core web app in Azure](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

## <a name="publish-with-msdeploy-on-windows"></a><span data-ttu-id="63246-150">Windows üzerinde MSDeploy ile yayımlama</span><span class="sxs-lookup"><span data-stu-id="63246-150">Publish with MSDeploy on Windows</span></span>

<span data-ttu-id="63246-151">Bkz: <xref:host-and-deploy/visual-studio-publish-profiles> yayımlama profilini bir Visual Studio ile uygulama yayımlama konusunda yönergeler için kullanarak bir Windows komut istemi dahil olmak üzere [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) komutu.</span><span class="sxs-lookup"><span data-stu-id="63246-151">See <xref:host-and-deploy/visual-studio-publish-profiles> for instructions on how to publish an app with a Visual Studio publish profile, including from a Windows command prompt using the [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command.</span></span>

## <a name="host-in-a-web-farm"></a><span data-ttu-id="63246-152">Web grubunda barındırma</span><span class="sxs-lookup"><span data-stu-id="63246-152">Host in a web farm</span></span>

<span data-ttu-id="63246-153">(Örneğin, uygulamanız ölçeklenebilirlik için birden çok örneğini dağıtımı) web grubu ortamında ASP.NET Core uygulamaları barındırmak için yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/web-farm>.</span><span class="sxs-lookup"><span data-stu-id="63246-153">For information on configuration for hosting ASP.NET Core apps in a web farm environment (for example, deployment of multiple instances of your app for scalability), see <xref:host-and-deploy/web-farm>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="perform-health-checks"></a><span data-ttu-id="63246-154">Sistem durumu denetimleri gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="63246-154">Perform health checks</span></span>

<span data-ttu-id="63246-155">Sistem durumu denetleme ara yazılım, bir uygulamayı ve bağımlılıklarını sistem durumu denetimleri gerçekleştirmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="63246-155">Use Health Check Middleware to perform health checks on an app and its dependencies.</span></span> <span data-ttu-id="63246-156">Daha fazla bilgi için bkz. <xref:host-and-deploy/health-checks>.</span><span class="sxs-lookup"><span data-stu-id="63246-156">For more information, see <xref:host-and-deploy/health-checks>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="63246-157">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="63246-157">Additional resources</span></span>

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
