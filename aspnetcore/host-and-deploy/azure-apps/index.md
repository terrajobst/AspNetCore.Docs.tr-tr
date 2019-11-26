---
title: ASP.NET Core uygulamalarını Azure App Service dağıtma
author: bradygaster
description: Bu makale, Azure ana bilgisayarına bağlantılar ve kaynakları dağıtmak için bağlantı içerir.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/07/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: f9fc6e706046165c142e19ca38d97ac21914dc9b
ms.sourcegitcommit: a104ba258ae7c0b3ee7c6fa7eaea1ddeb8b6eb73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74478768"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="9d7ed-103">ASP.NET Core uygulamalarını Azure App Service dağıtma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="9d7ed-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Web uygulamalarını barındırmak için ASP.NET Core dahil olmak üzere bir [Microsoft bulut bilgi işlem platformu hizmetidir](https://azure.microsoft.com/) .</span><span class="sxs-lookup"><span data-stu-id="9d7ed-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="9d7ed-105">Faydalı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9d7ed-105">Useful resources</span></span>

<span data-ttu-id="9d7ed-106">[App Service belge](/azure/app-service/) , Azure Apps belgelerinin, öğreticilerin, örneklerin, nasıl yapılır kılavuzlarının ve diğer kaynakların ana adresidir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="9d7ed-107">Barındırma ASP.NET Core uygulamaları ile ilgili iki önemli öğretici şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="9d7ed-108">Azure 'da ASP.NET Core Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="9d7ed-109">ASP.NET Core bir Web uygulamasını Windows üzerinde Azure App Service oluşturmak ve dağıtmak için Visual Studio 'Yu kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="9d7ed-110">Linux üzerinde App Service ASP.NET Core uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="9d7ed-111">Linux üzerinde Azure App Service için bir ASP.NET Core Web uygulaması oluşturup dağıtmak üzere komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="9d7ed-112">Azure App Service 'te bulunan ASP.NET Core sürümü için [App Service panosundaki ASP.NET Core](https://aspnetcoreon.azurewebsites.net/) bakın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-112">See the [ASP.NET Core on App Service Dashboard](https://aspnetcoreon.azurewebsites.net/) for the version of ASP.NET Core available on Azure App service.</span></span>

<span data-ttu-id="9d7ed-113">[App Service Duyurular](https://github.com/Azure/app-service-announcements/) deposuna abone olun ve sorunları izleyin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-113">Subscribe to the [App Service Announcements](https://github.com/Azure/app-service-announcements/) repository and monitor the issues.</span></span> <span data-ttu-id="9d7ed-114">App Service takım, App Service gelen duyuruları ve senaryoları düzenli olarak gönderir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-114">The App Service team regularly posts announcements and scenarios arriving in App Service.</span></span>

<span data-ttu-id="9d7ed-115">Aşağıdaki makaleler ASP.NET Core belgelerinde sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-115">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="9d7ed-116">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-116">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="9d7ed-117">Visual Studio kullanarak ASP.NET Core bir Web uygulaması oluşturmayı ve sürekli dağıtım için git 'i kullanarak Azure App Service nasıl dağıtacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-117">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="9d7ed-118">İlk işlem hattınızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-118">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="9d7ed-119">ASP.NET Core bir uygulama için CI derlemesi ayarlayın ve Azure App Service için sürekli bir dağıtım sürümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-119">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="9d7ed-120">Azure Web uygulaması korumalı alanı</span><span class="sxs-lookup"><span data-stu-id="9d7ed-120">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="9d7ed-121">Azure Apps platformu tarafından zorlanan Azure App Service çalışma zamanı yürütme sınırlamalarını bulun.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-121">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

<xref:test/troubleshoot>  
<span data-ttu-id="9d7ed-122">ASP.NET Core projelerle uyarıları ve hataları anlayın ve sorun giderin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-122">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="9d7ed-123">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="9d7ed-123">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="9d7ed-124">Platform</span><span class="sxs-lookup"><span data-stu-id="9d7ed-124">Platform</span></span>

<span data-ttu-id="9d7ed-125">Bir App Services uygulamasının platform mimarisi (x86/x64), A serisi bir işlem (temel) veya daha yüksek bir barındırma katmanında barındırılan uygulamalar için Azure portalında uygulama ayarlarında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-125">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute (Basic) or higher hosting tier.</span></span> <span data-ttu-id="9d7ed-126">Uygulamanın yayımlama ayarlarının (örneğin, Visual Studio [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)) Azure portalındaki uygulamanın hizmet yapılandırmasındaki ayarla eşleştiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-126">Confirm that the app's publish settings (for example, in the Visual Studio [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)) match the setting in the app's service configuration in the Azure Portal.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9d7ed-127">64-bit (x64) ve 32-bit (x86) uygulamalarının çalışma zamanları Azure App Service vardır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-127">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="9d7ed-128">App Service kullanılabilir [.NET Core SDK](/dotnet/core/sdk) 32 bittir, ancak [kudu](https://github.com/projectkudu/kudu/wiki) konsolunu veya Visual Studio 'daki Yayımla işlemini kullanarak yerel olarak oluşturulan 64 bit uygulamaları dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-128">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="9d7ed-129">Daha fazla bilgi için, [uygulamayı yayımlama ve dağıtma](#publish-and-deploy-the-app) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-129">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="9d7ed-130">Yerel bağımlılıklara sahip uygulamalar için, 32-bit (x86) uygulamalarının çalışma zamanları Azure App Service vardır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-130">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="9d7ed-131">App Service kullanılabilir [.NET Core SDK](/dotnet/core/sdk) 32 bitlik bir değer.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-131">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="9d7ed-132">.NET Core Framework bileşenleri ve dağıtım yöntemleri hakkında daha fazla bilgi için, .NET Core çalışma zamanı ve .NET Core SDK hakkında bilgi gibi bkz. [.NET Core: bileşim hakkında](/dotnet/core/about#composition).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-132">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="9d7ed-133">Paketler</span><span class="sxs-lookup"><span data-stu-id="9d7ed-133">Packages</span></span>

<span data-ttu-id="9d7ed-134">Azure App Service dağıtılan uygulamalar için otomatik günlük oluşturma özellikleri sağlamak üzere aşağıdaki NuGet paketlerini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-134">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="9d7ed-135">[Microsoft. AspNetCore. AzureAppServices. hostingstartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) , Azure App Service ile ASP.NET Core hafif tümleştirme sağlamak Için [ıhostingstartup](xref:fundamentals/configuration/platform-specific-configuration) kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-135">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="9d7ed-136">Eklenen günlük özellikleri `Microsoft.AspNetCore.AzureAppServicesIntegration` paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-136">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="9d7ed-137">[Microsoft. AspNetCore. AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) , `Microsoft.Extensions.Logging.AzureAppServices` paketine Azure App Service tanılama günlüğü sağlayıcıları eklemek Için [Addavzurewebappdiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) ' i yürütür.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-137">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="9d7ed-138">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) , Azure App Service tanılama günlüklerini ve günlük akışı özelliklerini desteklemek için günlükçü uygulamaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-138">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="9d7ed-139">Önceki paketlere [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)tarafından ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-139">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="9d7ed-140">`Microsoft.AspNetCore.App` metapackage .NET Framework veya başvurusunu hedefleyen uygulamalar, uygulamanın proje dosyasındaki ayrı paketlere açık olarak başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-140">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="9d7ed-141">Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-141">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="9d7ed-142">Azure portalındaki uygulama ayarları, uygulamanın ortam değişkenlerini ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-142">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="9d7ed-143">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider)tarafından tüketilebilir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-143">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="9d7ed-144">Azure portalında bir uygulama ayarı oluşturulduğunda veya değiştirildiğinde ve **Kaydet** düğmesi seçildiğinde, Azure uygulaması yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-144">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="9d7ed-145">Ortam değişkeni, hizmet yeniden başlatıldıktan sonra uygulama için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-145">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d7ed-146">Bir uygulama [genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullandığında, ortam değişkenleri varsayılan olarak bir uygulamanın yapılandırmasına yüklenmez ve yapılandırma sağlayıcısı geliştirici tarafından eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-146">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="9d7ed-147">Geliştirici, yapılandırma sağlayıcısı eklendiğinde ortam değişkeni önekini belirler.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-147">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="9d7ed-148">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host> ve [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-148">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d7ed-149">Bir uygulama, [Webhost. CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)kullanarak Konağı oluşturduğunda, Konağı yapılandıran ortam değişkenleri `ASPNETCORE_` önekini kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-149">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="9d7ed-150">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host> ve [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-150">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="9d7ed-151">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="9d7ed-151">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="9d7ed-152">[İşlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model)barındırma sırasında Iletilen üstbilgiler ara yazılımını yapılandıran ve ASP.NET Core modülü DÜZENI (http/https) ve isteğin KAYNAKLANDıĞı uzak IP adresini iletecek şekilde yapılandırılmış [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-152">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="9d7ed-153">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-153">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="9d7ed-154">Daha fazla bilgi için bkz. [proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-154">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="9d7ed-155">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="9d7ed-155">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d7ed-156">App Service dağıtılan ASP.NET Core uygulamalar, bir App Service uzantısı **ASP.NET Core günlüğe kaydetme tümleştirmesi**otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-156">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="9d7ed-157">Uzantı, Azure App Service ASP.NET Core uygulamalar için günlüğe kaydetme tümleştirmesi imkanı sunar.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-157">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d7ed-158">App Service dağıtılan ASP.NET Core uygulamalar, bir App Service uzantısı **ASP.NET Core günlüğe kaydetme uzantısı**otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-158">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="9d7ed-159">Uzantı, Azure App Service ASP.NET Core uygulamalar için günlüğe kaydetme tümleştirmesi imkanı sunar.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-159">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="9d7ed-160">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-160">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="9d7ed-161">Azure App Service uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="9d7ed-161">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="9d7ed-162">Uygulamalar ve App Service planları için kotaları ve ölçümleri incelemeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-162">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="9d7ed-163">Azure App Service uygulamalar için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="9d7ed-163">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="9d7ed-164">HTTP durum kodları, başarısız istekler ve Web sunucusu etkinliği için tanılama günlüğü 'nün nasıl etkinleştirileceğini ve erişebileceğini öğrenin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-164">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="9d7ed-165">ASP.NET Core uygulamalarında hataları işlemeye yönelik yaygın yaklaşımları anlayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-165">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="9d7ed-166">ASP.NET Core uygulamalarla Azure App Service dağıtımlarla ilgili sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-166">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="9d7ed-167">Sorun giderme önerisi ile Azure App Service/IIS tarafından barındırılan uygulamalar için ortak dağıtım yapılandırma hatalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-167">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="9d7ed-168">Veri koruma anahtar halkası ve dağıtım Yuvaları</span><span class="sxs-lookup"><span data-stu-id="9d7ed-168">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="9d7ed-169">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) *%Home%\ASP.NET\DataProtection-Keys* klasöründe kalıcı hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-169">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="9d7ed-170">Bu klasör, ağ depolama tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-170">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="9d7ed-171">Anahtarlar bekleyen bir şekilde korunmuyor.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-171">Keys aren't protected at rest.</span></span> <span data-ttu-id="9d7ed-172">Bu klasör, bir uygulamanın tüm örneklerine tek bir dağıtım yuvasında anahtar halkasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-172">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="9d7ed-173">Hazırlama ve üretim gibi ayrı dağıtım yuvaları, anahtar halkasını paylaşmaz.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-173">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="9d7ed-174">Dağıtım yuvaları arasında takas edildiğinde, veri koruma kullanan tüm sistem, önceki yuva içindeki anahtar halkasını kullanarak depolanan verilerin şifresini çözemeyecektir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-174">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="9d7ed-175">ASP.NET tanımlama bilgisi ara yazılımı, tanımlama bilgilerini korumak için veri koruma kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-175">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="9d7ed-176">Bu, kullanıcılara standart ASP.NET tanımlama bilgisi ara yazılımı kullanan bir uygulamanın oturumunu kapatmakta olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-176">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="9d7ed-177">Yuvada bağımsız bir anahtar halka çözümü için, bir dış anahtar halka sağlayıcısı kullanın, örneğin:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-177">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="9d7ed-178">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="9d7ed-178">Azure Blob Storage</span></span>
* <span data-ttu-id="9d7ed-179">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="9d7ed-179">Azure Key Vault</span></span>
* <span data-ttu-id="9d7ed-180">SQL Mağazası</span><span class="sxs-lookup"><span data-stu-id="9d7ed-180">SQL store</span></span>
* <span data-ttu-id="9d7ed-181">Redsıs önbelleği</span><span class="sxs-lookup"><span data-stu-id="9d7ed-181">Redis cache</span></span>

<span data-ttu-id="9d7ed-182">Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-182">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>
<a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>

## <a name="deploy-aspnet-core-30-to-azure-app-service"></a><span data-ttu-id="9d7ed-183">Azure App Service ASP.NET Core 3,0 dağıtma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-183">Deploy ASP.NET Core 3.0 to Azure App Service</span></span>

<span data-ttu-id="9d7ed-184">ASP.NET Core 3,0 Azure App Service desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-184">ASP.NET Core 3.0 is supported on Azure App Service.</span></span> <span data-ttu-id="9d7ed-185">.NET Core 3,0 ' den sonraki bir .NET Core sürümünün önizleme sürümünü dağıtmak için, aşağıdaki tekniklerden birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-185">To deploy a preview release of a .NET Core version later than .NET Core 3.0, use one of the following techniques.</span></span> <span data-ttu-id="9d7ed-186">Bu yaklaşımlar, çalışma zamanı kullanılabilir ancak SDK Azure App Service üzerine yüklenmemişse de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-186">These approaches are also used when the runtime is available but the SDK hasn't been installed on Azure App Service.</span></span>

* [<span data-ttu-id="9d7ed-187">Azure Pipelines kullanarak .NET Core SDK sürümünü belirtin</span><span class="sxs-lookup"><span data-stu-id="9d7ed-187">Specify the .NET Core SDK Version using Azure Pipelines</span></span>](#specify-the-net-core-sdk-version-using-azure-pipelines)
* <span data-ttu-id="9d7ed-188">[Kendi kendine içerilen bir önizleme uygulaması dağıtın](#deploy-a-self-contained-preview-app).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-188">[Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).</span></span>
* <span data-ttu-id="9d7ed-189">[Kapsayıcılar için Web Apps Docker kullanın](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-189">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>
* <span data-ttu-id="9d7ed-190">[Önizleme sitesi uzantısını yükler](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-190">[Install the preview site extension](#install-the-preview-site-extension).</span></span>

### <a name="specify-the-net-core-sdk-version-using-azure-pipelines"></a><span data-ttu-id="9d7ed-191">Azure Pipelines kullanarak .NET Core SDK sürümünü belirtin</span><span class="sxs-lookup"><span data-stu-id="9d7ed-191">Specify the .NET Core SDK Version using Azure Pipelines</span></span>

<span data-ttu-id="9d7ed-192">Azure DevOps ile sürekli tümleştirme derlemesi ayarlamak için [Azure App SERVICE CI/CD senaryoları](/azure/app-service/deploy-continuous-deployment) kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-192">Use [Azure App Service CI/CD scenarios](/azure/app-service/deploy-continuous-deployment) to set up a continuous integration build with Azure DevOps.</span></span> <span data-ttu-id="9d7ed-193">Azure DevOps derlemesi oluşturulduktan sonra, isteğe bağlı olarak derlemeyi belirli bir SDK sürümünü kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-193">After the Azure DevOps build is created, optionally configure the build to use a specific SDK version.</span></span> 

#### <a name="specify-the-net-core-sdk-version"></a><span data-ttu-id="9d7ed-194">.NET Core SDK sürümünü belirtin</span><span class="sxs-lookup"><span data-stu-id="9d7ed-194">Specify the .NET Core SDK version</span></span>

<span data-ttu-id="9d7ed-195">Azure DevOps derlemesi oluşturmak için App Service dağıtım merkezini kullanırken, varsayılan derleme işlem hattı `Restore`, `Build`, `Test`ve `Publish`için adımlar içerir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-195">When using the App Service deployment center to create an Azure DevOps build, the default build pipeline includes steps for `Restore`, `Build`, `Test`, and `Publish`.</span></span> <span data-ttu-id="9d7ed-196">SDK sürümünü belirtmek için, yeni bir adım eklemek üzere aracı iş listesindeki **Ekle (+)** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-196">To specify the SDK version, select the **Add (+)** button in the Agent job list to add a new step.</span></span> <span data-ttu-id="9d7ed-197">Arama çubuğunda **.NET Core SDK** arayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-197">Search for **.NET Core SDK** in the search bar.</span></span> 

![.NET Core SDK adımını ekleyin](index/add-sdk-step.png)

<span data-ttu-id="9d7ed-199">Aşağıdaki adımların .NET Core SDK belirtilen sürümünü kullanmasını sağlamak için adımı derlemedeki ilk konuma taşıyın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-199">Move the step into the first position in the build so that the steps following it use the specified version of the .NET Core SDK.</span></span> <span data-ttu-id="9d7ed-200">.NET Core SDK sürümünü belirtin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-200">Specify the version of the .NET Core SDK.</span></span> <span data-ttu-id="9d7ed-201">Bu örnekte, SDK `3.0.100`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-201">In this example, the SDK is set to `3.0.100`.</span></span>

![SDK adımı tamamlandı](index/sdk-step-first-place.png)

<span data-ttu-id="9d7ed-203">[Kendi kendine içerilen bir dağıtımı (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)yayımlamak için `Publish` adımında SCD ' yi yapılandırın ve [çalışma zamanı tanımlayıcısını (RID)](/dotnet/core/rid-catalog)sağlayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-203">To publish a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd), configure SCD in the `Publish` step and provide the [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

![Kendi içinde yayımlama](index/self-contained.png)

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="9d7ed-205">Kendi içinde bir önizleme uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-205">Deploy a self-contained preview app</span></span>

<span data-ttu-id="9d7ed-206">Bir önizleme çalışma zamanını hedefleyen [kendinden bağımsız bir dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) , dağıtımdaki Önizleme çalışma zamanını taşır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-206">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="9d7ed-207">Kendi kendine içerilen bir uygulama dağıtımında:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-207">When deploying a self-contained app:</span></span>

* <span data-ttu-id="9d7ed-208">Azure App Service sitesi, [Önizleme sitesi uzantısını](#install-the-preview-site-extension)gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-208">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="9d7ed-209">Uygulamanın, [çerçeveye bağımlı dağıtım (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd)için yayımlarken farklı bir yaklaşımdan sonra yayımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-209">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="9d7ed-210">[Uygulamanın kendi Içinde dağıtımı](#deploy-the-app-self-contained) bölümünde yer alan yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-210">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="9d7ed-211">Kapsayıcılar için Web Apps Docker kullanma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-211">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="9d7ed-212">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) , en son önizleme Docker görüntülerini içerir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-212">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="9d7ed-213">Görüntüler, temel görüntü olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-213">The images can be used as a base image.</span></span> <span data-ttu-id="9d7ed-214">Görüntüyü kullanın ve kapsayıcılar için normal olarak Web Apps için dağıtın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-214">Use the image and deploy to Web Apps for Containers normally.</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="9d7ed-215">Önizleme sitesi uzantısını yükler</span><span class="sxs-lookup"><span data-stu-id="9d7ed-215">Install the preview site extension</span></span>

<span data-ttu-id="9d7ed-216">Önizleme sitesi uzantısı kullanılarak bir sorun oluşursa, bir [ASPNET/AspNetCore sorunu](https://github.com/aspnet/AspNetCore/issues)açın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-216">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="9d7ed-217">Azure portalından App Service gidin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-217">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="9d7ed-218">Web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-218">Select the web app.</span></span>
1. <span data-ttu-id="9d7ed-219">"Uzantıları" filtrelemek için arama kutusuna "Ex" yazın veya yönetim araçları listesini aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-219">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="9d7ed-220">**Uzantıları**seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-220">Select **Extensions**.</span></span>
1. <span data-ttu-id="9d7ed-221">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-221">Select **Add**.</span></span>
1. <span data-ttu-id="9d7ed-222">Listeden `{X.Y}` ASP.NET Core önizleme sürümü olduğu ve `{x64|x86}` platformu belirten **ASP.NET Core {X. Y} ({x64 | x86}) çalışma zamanı** uzantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-222">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="9d7ed-223">Yasal koşulları kabul etmek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-223">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="9d7ed-224">Uzantıyı yüklemek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-224">Select **OK** to install the extension.</span></span>

<span data-ttu-id="9d7ed-225">İşlem tamamlandığında, en son .NET Core önizlemesi yüklenir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-225">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="9d7ed-226">Yüklemeyi doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-226">Verify the installation:</span></span>

1. <span data-ttu-id="9d7ed-227">**Gelişmiş Araçlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-227">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="9d7ed-228">**Gelişmiş araçlarda** **Git** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-228">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="9d7ed-229">**Hata ayıklama konsolu** > **PowerShell** menü öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-229">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="9d7ed-230">PowerShell komut isteminde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-230">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="9d7ed-231">`{X.Y}` için ASP.NET Core çalışma zamanı sürümünü ve komutuna `{PLATFORM}` platformunu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-231">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="9d7ed-232">X64 Önizleme çalışma zamanı yüklendiğinde komut `True` döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-232">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="9d7ed-233">Bir App Services uygulamasının platform mimarisi (x86/x64), A serisi bir işlem (temel) veya daha yüksek bir barındırma katmanında barındırılan uygulamalar için Azure portalında uygulama ayarlarında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-233">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute (Basic) or higher hosting tier.</span></span> <span data-ttu-id="9d7ed-234">Uygulamanın yayımlama ayarlarının (örneğin, Visual Studio [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)) Azure Portal uygulamanın hizmet yapılandırmasındaki ayarla eşleştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-234">Confirm that the app's publish settings (for example, in the Visual Studio [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)) match the setting in the app's service configuration in the Azure portal.</span></span>
>
> <span data-ttu-id="9d7ed-235">Uygulama, işlem içi modda çalışıyorsa ve platform mimarisi 64-bit (x64) için yapılandırılmışsa ASP.NET Core modülü, varsa 64 bit önizleme çalışma zamanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-235">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="9d7ed-236">Azure portalını kullanarak **ASP.NET Core {X. Y} (x64) çalışma zamanı** uzantısını yükler.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-236">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension using the Azure Portal.</span></span>
>
> <span data-ttu-id="9d7ed-237">X64 Önizleme çalışma zamanını yükledikten sonra, yüklemeyi doğrulamak için Azure kudu PowerShell komut penceresinde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-237">After installing the x64 preview runtime, run the following command in the Azure Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="9d7ed-238">Aşağıdaki komutta `{X.Y}` için ASP.NET Core çalışma zamanı sürümünü değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-238">Substitute the ASP.NET Core runtime version for `{X.Y}` in the following command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="9d7ed-239">X64 Önizleme çalışma zamanı yüklendiğinde komut `True` döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-239">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="9d7ed-240">**ASP.NET Core uzantıları** , Azure Uygulama Hizmetleri 'nde Azure günlük kaydı etkinleştirme gibi ek ASP.NET Core işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-240">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="9d7ed-241">Uzantı Visual Studio 'dan dağıtıldığında otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-241">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="9d7ed-242">Uzantı yüklü değilse, uygulama için bu uygulamayı yükleme.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-242">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="9d7ed-243">**Bir ARM şablonuyla önizleme sitesi uzantısını kullanma**</span><span class="sxs-lookup"><span data-stu-id="9d7ed-243">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="9d7ed-244">Uygulamaları oluşturmak ve dağıtmak için bir ARM şablonu kullanılıyorsa, `siteextensions` kaynak türü, site uzantısını bir Web uygulamasına eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-244">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="9d7ed-245">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-245">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="9d7ed-246">Uygulamayı yayımlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-246">Publish and deploy the app</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9d7ed-247">64 bitlik bir dağıtım için:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-247">For a 64-bit deployment:</span></span>

* <span data-ttu-id="9d7ed-248">64 bit uygulama derlemek için 64 bit .NET Core SDK kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-248">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="9d7ed-249">App Service **yapılandırma** > **genel ayarları**'nda **platformu** **64 bit** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-249">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="9d7ed-250">Uygulamanın, platform bit özelliğini tercih etmek için temel veya daha yüksek bir hizmet planı kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-250">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="9d7ed-251">Uygulama çerçevesine bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="9d7ed-251">Deploy the app framework-dependent</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d7ed-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d7ed-252">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="9d7ed-253">Visual Studio araç çubuğundan **derleme** >  **{Application Name} Yayımla** ' yı seçin veya **Çözüm Gezgini** projeye sağ tıklayıp **Yayımla**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-253">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="9d7ed-254">**Bir yayımlama hedefi seç** iletişim kutusunda **App Service** seçili olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-254">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="9d7ed-255">**Gelişmiş**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-255">Select **Advanced**.</span></span> <span data-ttu-id="9d7ed-256">**Yayımla** iletişim kutusu açılır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-256">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="9d7ed-257">**Yayımla** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-257">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="9d7ed-258">**Yayın** yapılandırmasının seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-258">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="9d7ed-259">**Dağıtım modu** açılır listesini açın ve **çerçeveye bağımlı**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-259">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="9d7ed-260">**Hedef çalışma zamanı**olarak **Taşınabilir** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-260">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="9d7ed-261">Dağıtımdan sonra ek dosyaları kaldırmanız gerekiyorsa, **dosya yayımlama seçenekleri** ' ni açın ve hedefteki ek dosyaları kaldırmak için onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-261">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="9d7ed-262">**Kaydet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-262">Select **Save**.</span></span>
1. <span data-ttu-id="9d7ed-263">Yayımla sihirbazının kalan istemlerini izleyerek yeni bir site oluşturun veya var olan bir siteyi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-263">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9d7ed-264">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9d7ed-264">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="9d7ed-265">Proje dosyasında, bir [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog)belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-265">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="9d7ed-266">Bir komut kabuğundan, uygulamayı sürüm yapılandırmasında [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutuyla yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-266">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="9d7ed-267">Aşağıdaki örnekte, uygulama çerçeveye bağlı bir uygulama olarak yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-267">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="9d7ed-268">*Bin/Release/{Target Framework}/Publish* dizininin içeriğini App Service sitesinde siteye taşıyın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-268">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="9d7ed-269">*Klasör içeriğini* yerel sabit sürücünüzden veya ağ paylaşımınızdan, [kudu](https://github.com/projectkudu/kudu/wiki) konsolundaki App Service doğrudan sürüklerseniz, dosyaları kudu konsolundaki `D:\home\site\wwwroot` klasörüne sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-269">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="9d7ed-270">Uygulamayı kendi içinde dağıtma</span><span class="sxs-lookup"><span data-stu-id="9d7ed-270">Deploy the app self-contained</span></span>

<span data-ttu-id="9d7ed-271">Kendi içinde dağıtım için Visual Studio 'Yu veya komut satırı arabirimi (CLı) araçlarını kullanın [(SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-271">Use Visual Studio or the command-line interface (CLI) tools for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d7ed-272">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d7ed-272">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="9d7ed-273">Visual Studio araç çubuğundan **derleme** >  **{Application Name} Yayımla** ' yı seçin veya **Çözüm Gezgini** projeye sağ tıklayıp **Yayımla**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-273">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="9d7ed-274">**Bir yayımlama hedefi seç** iletişim kutusunda **App Service** seçili olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-274">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="9d7ed-275">**Gelişmiş**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-275">Select **Advanced**.</span></span> <span data-ttu-id="9d7ed-276">**Yayımla** iletişim kutusu açılır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-276">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="9d7ed-277">**Yayımla** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-277">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="9d7ed-278">**Yayın** yapılandırmasının seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-278">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="9d7ed-279">**Dağıtım modu** açılır listesini açın ve **kendinden bağımsız**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-279">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="9d7ed-280">Hedef **çalışma** zamanı açılır listesinden hedef çalışma zamanını seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-280">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="9d7ed-281">Varsayılan, `win-x86` değeridir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-281">The default is `win-x86`.</span></span>
   * <span data-ttu-id="9d7ed-282">Dağıtımdan sonra ek dosyaları kaldırmanız gerekiyorsa, **dosya yayımlama seçenekleri** ' ni açın ve hedefteki ek dosyaları kaldırmak için onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-282">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="9d7ed-283">**Kaydet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-283">Select **Save**.</span></span>
1. <span data-ttu-id="9d7ed-284">Yayımla sihirbazının kalan istemlerini izleyerek yeni bir site oluşturun veya var olan bir siteyi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-284">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9d7ed-285">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9d7ed-285">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="9d7ed-286">Proje dosyasında bir veya daha fazla [çalışma zamanı tanımlayıcısı (RID 'ler)](/dotnet/core/rid-catalog)belirtin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-286">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="9d7ed-287">Tek bir RID için `<RuntimeIdentifier>` (tekil) kullanın veya noktalı virgülle ayrılmış RID listesi sağlamak için `<RuntimeIdentifiers>` (plural) kullanın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-287">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="9d7ed-288">Aşağıdaki örnekte, `win-x86` RID belirtilir:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-288">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="9d7ed-289">Bir komut kabuğundan, uygulamayı, [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutuyla konağın çalışma zamanının sürüm yapılandırmasında yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-289">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="9d7ed-290">Aşağıdaki örnekte, uygulama `win-x86` RID için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-290">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="9d7ed-291">`--runtime` seçeneğine sağlanan RID, proje dosyasındaki `<RuntimeIdentifier>` (veya `<RuntimeIdentifiers>`) özelliğinde sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-291">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86 --self-contained
   ```

1. <span data-ttu-id="9d7ed-292">*Bin/Release/{Target Framework}/{RUNTIME Identifier}/Publish* dizininin içeriğini App Service sitesinde siteye taşıyın.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-292">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="9d7ed-293">*Klasör içeriğini* yerel sabit sürücünüzden veya ağ paylaşımınızdan, kudu konsolundaki App Service doğrudan sürüklerseniz, dosyaları kudu konsolundaki `D:\home\site\wwwroot` klasörüne sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-293">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="9d7ed-294">Protokol ayarları (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="9d7ed-294">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="9d7ed-295">Güvenli Protokol bağlamaları, HTTPS üzerinden isteklere yanıt verme sırasında kullanılacak bir sertifika belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-295">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="9d7ed-296">Bağlama, belirli bir ana bilgisayar adı için verilen geçerli bir özel sertifika ( *. pfx*) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-296">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="9d7ed-297">Daha fazla bilgi için bkz. [öğretici: var olan bir özel SSL sertifikasını Azure App Service bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="9d7ed-297">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="9d7ed-298">Web.config’i dönüştürme</span><span class="sxs-lookup"><span data-stu-id="9d7ed-298">Transform web.config</span></span>

<span data-ttu-id="9d7ed-299">Yayımlama sırasında *Web. config* ' i dönüştürmeniz gerekiyorsa (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlayın), bkz. <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-299">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d7ed-300">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9d7ed-300">Additional resources</span></span>

* [<span data-ttu-id="9d7ed-301">App Service Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="9d7ed-301">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="9d7ed-302">Azure App Service: .NET uygulamalarınızı barındırmak için En Iyi yer (55 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="9d7ed-302">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="9d7ed-303">Azure Cuma: Azure App Service tanılama ve sorun giderme deneyimi (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="9d7ed-303">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="9d7ed-304">Azure App Service tanılamada genel bakış</span><span class="sxs-lookup"><span data-stu-id="9d7ed-304">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="9d7ed-305">Windows Server 'da Azure App Service, [Internet Information Services (IIS)](https://www.iis.net/)kullanır.</span><span class="sxs-lookup"><span data-stu-id="9d7ed-305">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="9d7ed-306">Aşağıdaki konular, temel alınan IIS teknolojisine yöneliktir:</span><span class="sxs-lookup"><span data-stu-id="9d7ed-306">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="9d7ed-307">Windows Server-geçerli ve önceki sürümler için BT Yöneticisi içeriği</span><span class="sxs-lookup"><span data-stu-id="9d7ed-307">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
