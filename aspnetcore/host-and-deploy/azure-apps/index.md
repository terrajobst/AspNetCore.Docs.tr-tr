---
title: ASP.NET Core uygulamalarını Azure App Service dağıtma
author: guardrex
description: Bu makale, Azure ana bilgisayarına bağlantılar ve kaynakları dağıtmak için bağlantı içerir.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/28/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 5035a31526e0290964e0fdee05753aeaf6cb3790
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602445"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="7e0b4-103">ASP.NET Core uygulamalarını Azure App Service dağıtma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="7e0b4-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) , Web uygulamalarını barındırmak için ASP.NET Core dahil olmak üzere bir [Microsoft bulut bilgi işlem platformu hizmetidir](https://azure.microsoft.com/) .</span><span class="sxs-lookup"><span data-stu-id="7e0b4-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="7e0b4-105">Faydalı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7e0b4-105">Useful resources</span></span>

<span data-ttu-id="7e0b4-106">[App Service belge](/azure/app-service/) , Azure Apps belgelerinin, öğreticilerin, örneklerin, nasıl yapılır kılavuzlarının ve diğer kaynakların ana adresidir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="7e0b4-107">Barındırma ASP.NET Core uygulamaları ile ilgili iki önemli öğretici şunlardır:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="7e0b4-108">Azure 'da ASP.NET Core Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="7e0b4-109">ASP.NET Core bir Web uygulamasını Windows üzerinde Azure App Service oluşturmak ve dağıtmak için Visual Studio 'Yu kullanın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="7e0b4-110">Linux üzerinde App Service ASP.NET Core uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="7e0b4-111">Linux üzerinde Azure App Service için bir ASP.NET Core Web uygulaması oluşturup dağıtmak üzere komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="7e0b4-112">Aşağıdaki makaleler ASP.NET Core belgelerinde sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="7e0b4-113">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="7e0b4-114">Visual Studio kullanarak ASP.NET Core bir Web uygulaması oluşturmayı ve sürekli dağıtım için git 'i kullanarak Azure App Service nasıl dağıtacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="7e0b4-115">İlk işlem hattınızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-115">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="7e0b4-116">ASP.NET Core bir uygulama için CI derlemesi ayarlayın ve Azure App Service için sürekli bir dağıtım sürümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="7e0b4-117">Azure Web uygulaması korumalı alanı</span><span class="sxs-lookup"><span data-stu-id="7e0b4-117">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="7e0b4-118">Azure Apps platformu tarafından zorlanan Azure App Service çalışma zamanı yürütme sınırlamalarını bulun.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

<xref:test/troubleshoot>  
<span data-ttu-id="7e0b4-119">ASP.NET Core projelerle uyarıları ve hataları anlayın ve sorun giderin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-119">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="7e0b4-120">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="7e0b4-120">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="7e0b4-121">Platform</span><span class="sxs-lookup"><span data-stu-id="7e0b4-121">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e0b4-122">64-bit (x64) ve 32-bit (x86) uygulamalarının çalışma zamanları Azure App Service vardır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-122">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="7e0b4-123">App Service kullanılabilir [.NET Core SDK](/dotnet/core/sdk) 32 bittir, ancak [kudu](https://github.com/projectkudu/kudu/wiki) konsolunu veya Visual Studio 'daki Yayımla işlemini kullanarak yerel olarak oluşturulan 64 bit uygulamaları dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-123">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="7e0b4-124">Daha fazla bilgi için, [uygulamayı yayımlama ve dağıtma](#publish-and-deploy-the-app) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-124">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7e0b4-125">Yerel bağımlılıklara sahip uygulamalar için, 32-bit (x86) uygulamalarının çalışma zamanları Azure App Service vardır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-125">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="7e0b4-126">App Service kullanılabilir [.NET Core SDK](/dotnet/core/sdk) 32 bitlik bir değer.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-126">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="7e0b4-127">.NET Core Framework bileşenleri ve dağıtım yöntemleri hakkında daha fazla bilgi için, .NET Core çalışma zamanı ve .NET Core SDK hakkında bilgi gibi bkz [. .NET Core hakkında: Oluşturma](/dotnet/core/about#composition).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-127">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="7e0b4-128">Paketler</span><span class="sxs-lookup"><span data-stu-id="7e0b4-128">Packages</span></span>

<span data-ttu-id="7e0b4-129">Azure App Service dağıtılan uygulamalar için otomatik günlük oluşturma özellikleri sağlamak üzere aşağıdaki NuGet paketlerini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-129">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="7e0b4-130">[Microsoft. AspNetCore. AzureAppServices. hostingstartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) , Azure App Service ile ASP.NET Core hafif tümleştirme sağlamak Için [ıhostingstartup](xref:fundamentals/configuration/platform-specific-configuration) kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-130">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="7e0b4-131">Eklenen günlük özellikleri, `Microsoft.AspNetCore.AzureAppServicesIntegration` paket tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-131">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="7e0b4-132">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-132">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="7e0b4-133">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) , Azure App Service tanılama günlüklerini ve günlük akışı özelliklerini desteklemek için günlükçü uygulamaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-133">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="7e0b4-134">Önceki paketlere [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)tarafından ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-134">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="7e0b4-135">`Microsoft.AspNetCore.App` Metapackage 'i .NET Framework veya başvurusunu hedefleyen uygulamalar, uygulamanın proje dosyasındaki ayrı paketlere açık olarak başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-135">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="7e0b4-136">Azure portalını kullanarak uygulama yapılandırmasını geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-136">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="7e0b4-137">Azure portalındaki uygulama ayarları, uygulamanın ortam değişkenlerini ayarlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-137">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="7e0b4-138">Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider)tarafından tüketilebilir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-138">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="7e0b4-139">Azure portalında bir uygulama ayarı oluşturulduğunda veya değiştirildiğinde ve **Kaydet** düğmesi seçildiğinde, Azure uygulaması yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-139">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="7e0b4-140">Ortam değişkeni, hizmet yeniden başlatıldıktan sonra uygulama için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-140">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7e0b4-141">Bir uygulama [genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullandığında, ortam değişkenleri varsayılan olarak bir uygulamanın yapılandırmasına yüklenmez ve yapılandırma sağlayıcısı geliştirici tarafından eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-141">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="7e0b4-142">Geliştirici, yapılandırma sağlayıcısı eklendiğinde ortam değişkeni önekini belirler.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-142">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="7e0b4-143">Daha fazla bilgi için bkz <xref:fundamentals/host/generic-host> . ve [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-143">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7e0b4-144">Bir uygulama, [Webhost. createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)kullanarak Konağı oluşturduğunda, Konağı yapılandıran ortam değişkenleri `ASPNETCORE_` ön eki kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-144">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="7e0b4-145">Daha fazla bilgi için bkz <xref:fundamentals/host/web-host> . ve [ortam değişkenleri yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-145">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="7e0b4-146">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="7e0b4-146">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="7e0b4-147">[İşlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model)barındırma sırasında Iletilen üstbilgiler ara yazılımını yapılandıran ve ASP.NET Core modülü DÜZENI (http/https) ve isteğin BAŞLATıLDıĞı uzak IP adresini iletecek şekilde yapılandırılmış [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components). .</span><span class="sxs-lookup"><span data-stu-id="7e0b4-147">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="7e0b4-148">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-148">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="7e0b4-149">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="7e0b4-150">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="7e0b4-150">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7e0b4-151">App Service dağıtılan ASP.NET Core uygulamalar, bir App Service uzantısı **ASP.NET Core günlüğe kaydetme tümleştirmesi**otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-151">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="7e0b4-152">Uzantı, Azure App Service ASP.NET Core uygulamalar için günlüğe kaydetme tümleştirmesi imkanı sunar.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-152">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7e0b4-153">App Service dağıtılan ASP.NET Core uygulamalar, bir App Service uzantısı **ASP.NET Core günlüğe kaydetme uzantısı**otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-153">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="7e0b4-154">Uzantı, Azure App Service ASP.NET Core uygulamalar için günlüğe kaydetme tümleştirmesi imkanı sunar.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-154">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="7e0b4-155">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-155">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="7e0b4-156">Azure App Service uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="7e0b4-156">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="7e0b4-157">Uygulamalar ve App Service planları için kotaları ve ölçümleri incelemeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-157">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="7e0b4-158">Azure App Service uygulamalar için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="7e0b4-158">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="7e0b4-159">HTTP durum kodları, başarısız istekler ve Web sunucusu etkinliği için tanılama günlüğü 'nün nasıl etkinleştirileceğini ve erişebileceğini öğrenin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-159">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="7e0b4-160">ASP.NET Core uygulamalarında hataları işlemeye yönelik yaygın yaklaşımları anlayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-160">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="7e0b4-161">ASP.NET Core uygulamalarla Azure App Service dağıtımlarla ilgili sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-161">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="7e0b4-162">Sorun giderme önerisi ile Azure App Service/IIS tarafından barındırılan uygulamalar için ortak dağıtım yapılandırma hatalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-162">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="7e0b4-163">Veri koruma anahtar halkası ve dağıtım Yuvaları</span><span class="sxs-lookup"><span data-stu-id="7e0b4-163">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="7e0b4-164">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) *%Home%\ASP.NET\DataProtection-Keys* klasöründe kalıcı hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-164">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="7e0b4-165">Bu klasör, ağ depolama tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-165">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="7e0b4-166">Anahtarlar bekleyen bir şekilde korunmuyor.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-166">Keys aren't protected at rest.</span></span> <span data-ttu-id="7e0b4-167">Bu klasör, bir uygulamanın tüm örneklerine tek bir dağıtım yuvasında anahtar halkasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-167">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="7e0b4-168">Hazırlama ve üretim gibi ayrı dağıtım yuvaları, anahtar halkasını paylaşmaz.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-168">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="7e0b4-169">Dağıtım yuvaları arasında takas edildiğinde, veri koruma kullanan tüm sistem, önceki yuva içindeki anahtar halkasını kullanarak depolanan verilerin şifresini çözemeyecektir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-169">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="7e0b4-170">ASP.NET tanımlama bilgisi ara yazılımı, tanımlama bilgilerini korumak için veri koruma kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-170">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="7e0b4-171">Bu, kullanıcılara standart ASP.NET tanımlama bilgisi ara yazılımı kullanan bir uygulamanın oturumunu kapatmakta olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-171">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="7e0b4-172">Yuvada bağımsız bir anahtar halka çözümü için, bir dış anahtar halka sağlayıcısı kullanın, örneğin:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-172">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="7e0b4-173">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="7e0b4-173">Azure Blob Storage</span></span>
* <span data-ttu-id="7e0b4-174">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7e0b4-174">Azure Key Vault</span></span>
* <span data-ttu-id="7e0b4-175">SQL Mağazası</span><span class="sxs-lookup"><span data-stu-id="7e0b4-175">SQL store</span></span>
* <span data-ttu-id="7e0b4-176">Redsıs önbelleği</span><span class="sxs-lookup"><span data-stu-id="7e0b4-176">Redis cache</span></span>

<span data-ttu-id="7e0b4-177">Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-177">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="7e0b4-178">Azure App Service için ASP.NET Core Preview sürümünü dağıtın</span><span class="sxs-lookup"><span data-stu-id="7e0b4-178">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="7e0b4-179">Uygulama .NET Core 'un önizleme sürümünü kullanıyorsa aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-179">Use one of the following approaches if the app relies on a preview release of .NET Core:</span></span>

* <span data-ttu-id="7e0b4-180">[Önizleme sitesi uzantısını yükler](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-180">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="7e0b4-181">[Kendi kendine içerilen bir önizleme uygulaması dağıtın](#deploy-a-self-contained-preview-app).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-181">[Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).</span></span>
* <span data-ttu-id="7e0b4-182">[Kapsayıcılar için Web Apps Docker kullanın](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-182">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="7e0b4-183">Önizleme sitesi uzantısını yükler</span><span class="sxs-lookup"><span data-stu-id="7e0b4-183">Install the preview site extension</span></span>

<span data-ttu-id="7e0b4-184">Önizleme sitesi uzantısı kullanılarak bir sorun oluşursa, bir [ASPNET/AspNetCore sorunu](https://github.com/aspnet/AspNetCore/issues)açın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-184">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="7e0b4-185">Azure portalından App Service gidin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-185">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="7e0b4-186">Web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-186">Select the web app.</span></span>
1. <span data-ttu-id="7e0b4-187">"Uzantıları" filtrelemek için arama kutusuna "Ex" yazın veya yönetim araçları listesini aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-187">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="7e0b4-188">**Uzantıları**seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-188">Select **Extensions**.</span></span>
1. <span data-ttu-id="7e0b4-189">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-189">Select **Add**.</span></span>
1. <span data-ttu-id="7e0b4-190">`{x64|x86}` Listeden`{X.Y}` ASP.NET Core önizleme sürümü olduğu ve platformu belirten **ASP.NET Core {X. Y} ({x64 | x86}) çalışma zamanı** uzantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-190">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="7e0b4-191">Yasal koşulları kabul etmek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-191">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="7e0b4-192">Uzantıyı yüklemek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-192">Select **OK** to install the extension.</span></span>

<span data-ttu-id="7e0b4-193">İşlem tamamlandığında, en son .NET Core önizlemesi yüklenir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-193">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="7e0b4-194">Yüklemeyi doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-194">Verify the installation:</span></span>

1. <span data-ttu-id="7e0b4-195">**Gelişmiş Araçlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-195">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="7e0b4-196">**Gelişmiş araçlarda** **Git** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-196">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="7e0b4-197">**Hata ayıklama konsolu** > **PowerShell** menü öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-197">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="7e0b4-198">PowerShell komut isteminde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-198">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="7e0b4-199">Komutu için `{X.Y}` ASP.NET Core çalışma zamanı sürümünü ve `{PLATFORM}` platformunu değiştirin:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-199">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="7e0b4-200">Bu komut x64 `True` önizlemesi çalışma zamanı yüklendiğinde döndürür.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-200">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="7e0b4-201">Uygulama hizmetleri uygulamasının platform mimarisi (x86/x64), bir dizi işlem veya daha iyi barındırma katmanında barındırılan uygulamalar için Azure portalındaki uygulama ayarlarında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-201">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="7e0b4-202">Uygulama, işlem içi modda çalışıyorsa ve platform mimarisi 64-bit (x64) için yapılandırılmışsa ASP.NET Core modülü, varsa 64 bit önizleme çalışma zamanını kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-202">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="7e0b4-203">**ASP.NET Core {X. Y} (x64) çalışma zamanı** uzantısını yükler.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-203">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="7e0b4-204">X64 Önizleme çalışma zamanını yükledikten sonra, yüklemenin doğrulanması için kudu PowerShell komut penceresinde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-204">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="7e0b4-205">Komutu için `{X.Y}` ASP.NET Core çalışma zamanı sürümünü yerine koyun:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-205">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="7e0b4-206">Bu komut x64 `True` önizlemesi çalışma zamanı yüklendiğinde döndürür.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-206">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="7e0b4-207">**ASP.NET Core uzantıları** , Azure Uygulama Hizmetleri 'nde Azure günlük kaydı etkinleştirme gibi ek ASP.NET Core işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-207">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="7e0b4-208">Uzantı Visual Studio 'dan dağıtıldığında otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-208">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="7e0b4-209">Uzantı yüklü değilse, uygulama için bu uygulamayı yükleme.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-209">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="7e0b4-210">**Bir ARM şablonuyla önizleme sitesi uzantısını kullanma**</span><span class="sxs-lookup"><span data-stu-id="7e0b4-210">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="7e0b4-211">Uygulama oluşturmak ve dağıtmak için bir ARM şablonu kullanılıyorsa, `siteextensions` kaynak türü bir Web uygulamasına site uzantısını eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-211">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="7e0b4-212">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-212">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="7e0b4-213">Kendi içinde bir önizleme uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-213">Deploy a self-contained preview app</span></span>

<span data-ttu-id="7e0b4-214">Bir önizleme çalışma zamanını hedefleyen [kendinden bağımsız bir dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) , dağıtımdaki Önizleme çalışma zamanını taşır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-214">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="7e0b4-215">Kendi kendine içerilen bir uygulama dağıtımında:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-215">When deploying a self-contained app:</span></span>

* <span data-ttu-id="7e0b4-216">Azure App Service sitesi, [Önizleme sitesi uzantısını](#install-the-preview-site-extension)gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-216">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="7e0b4-217">Uygulamanın, [çerçeveye bağımlı dağıtım (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd)için yayımlarken farklı bir yaklaşımdan sonra yayımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-217">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="7e0b4-218">[Uygulamanın kendi Içinde dağıtımı](#deploy-the-app-self-contained) bölümünde yer alan yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-218">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="7e0b4-219">Kapsayıcılar için Web Apps Docker kullanma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-219">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="7e0b4-220">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) , en son önizleme Docker görüntülerini içerir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-220">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="7e0b4-221">Görüntüler, temel görüntü olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-221">The images can be used as a base image.</span></span> <span data-ttu-id="7e0b4-222">Görüntüyü kullanın ve kapsayıcılar için normal olarak Web Apps için dağıtın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-222">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="7e0b4-223">Uygulamayı yayımlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-223">Publish and deploy the app</span></span>

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="7e0b4-224">Uygulama çerçevesine bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="7e0b4-224">Deploy the app framework-dependent</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e0b4-225">64 bitlik bir [çerçeveye bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)için:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-225">For a 64-bit [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

* <span data-ttu-id="7e0b4-226">64 bit uygulama derlemek için 64 bit .NET Core SDK kullanın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-226">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="7e0b4-227">App Service **yapılandırma**  genel > ayarlarında platformu **64 bit** olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-227">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="7e0b4-228">Uygulamanın, platform bit özelliğini tercih etmek için temel veya daha yüksek bir hizmet planı kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-228">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e0b4-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e0b4-229">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7e0b4-230">Visual Studio araç çubuğundan **derleme** > **yayımlaması {Application Name}** öğesini seçin veya **Çözüm Gezgini** ' de projeye sağ tıklayıp **Yayımla**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-230">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="7e0b4-231">**Bir yayımlama hedefi seç** iletişim kutusunda **App Service** seçili olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-231">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="7e0b4-232">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-232">Select **Advanced**.</span></span> <span data-ttu-id="7e0b4-233">**Yayımla** iletişim kutusu açılır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-233">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="7e0b4-234">İçinde **Yayımla** iletişim:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-234">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="7e0b4-235">**Yayın** yapılandırmasının seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-235">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="7e0b4-236">**Dağıtım modu** açılır listesini açın ve **çerçeveye bağımlı**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-236">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="7e0b4-237">**Hedef çalışma zamanı**olarak **Taşınabilir** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-237">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="7e0b4-238">Dağıtımdan sonra ek dosyaları kaldırmanız gerekiyorsa, **dosya yayımlama seçenekleri** ' ni açın ve hedefteki ek dosyaları kaldırmak için onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-238">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="7e0b4-239">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-239">Select **Save**.</span></span>
1. <span data-ttu-id="7e0b4-240">Yayımla sihirbazının kalan istemlerini izleyerek yeni bir site oluşturun veya var olan bir siteyi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-240">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7e0b4-241">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7e0b4-241">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="7e0b4-242">Proje dosyasında, bir [çalışma zamanı tanımlayıcısı (RID)](/dotnet/core/rid-catalog)belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-242">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="7e0b4-243">Bir komut kabuğundan, uygulamayı sürüm yapılandırmasında [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutuyla yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-243">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="7e0b4-244">Aşağıdaki örnekte, uygulama çerçeveye bağlı bir uygulama olarak yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-244">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="7e0b4-245">*Bin/Release/{Target Framework}/Publish* dizininin içeriğini App Service sitesinde siteye taşıyın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-245">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="7e0b4-246">*Klasör içeriğini* yerel sabit sürücünüzden veya ağ paylaşımınızdan, [kudu](https://github.com/projectkudu/kudu/wiki) konsolundaki App Service doğrudan sürüklerseniz, `D:\home\site\wwwroot` dosyaları kudu konsolundaki klasöre sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-246">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="7e0b4-247">Uygulamayı kendi içinde dağıtma</span><span class="sxs-lookup"><span data-stu-id="7e0b4-247">Deploy the app self-contained</span></span>

<span data-ttu-id="7e0b4-248">Kendi içinde dağıtım için Visual Studio 'Yu veya komut satırı arabirimi (CLı) araçlarını kullanın [(SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="7e0b4-248">Use Visual Studio or the command-line interface (CLI) tools for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7e0b4-249">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e0b4-249">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="7e0b4-250">Visual Studio araç çubuğundan **derleme** > **yayımlaması {Application Name}** öğesini seçin veya **Çözüm Gezgini** ' de projeye sağ tıklayıp **Yayımla**' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-250">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="7e0b4-251">**Bir yayımlama hedefi seç** iletişim kutusunda **App Service** seçili olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-251">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="7e0b4-252">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-252">Select **Advanced**.</span></span> <span data-ttu-id="7e0b4-253">**Yayımla** iletişim kutusu açılır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-253">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="7e0b4-254">İçinde **Yayımla** iletişim:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-254">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="7e0b4-255">**Yayın** yapılandırmasının seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-255">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="7e0b4-256">**Dağıtım modu** açılır listesini açın ve **kendinden bağımsız**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-256">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="7e0b4-257">Hedef **çalışma** zamanı açılır listesinden hedef çalışma zamanını seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-257">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="7e0b4-258">Varsayılan, `win-x86` değeridir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-258">The default is `win-x86`.</span></span>
   * <span data-ttu-id="7e0b4-259">Dağıtımdan sonra ek dosyaları kaldırmanız gerekiyorsa, **dosya yayımlama seçenekleri** ' ni açın ve hedefteki ek dosyaları kaldırmak için onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-259">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="7e0b4-260">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-260">Select **Save**.</span></span>
1. <span data-ttu-id="7e0b4-261">Yayımla sihirbazının kalan istemlerini izleyerek yeni bir site oluşturun veya var olan bir siteyi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-261">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7e0b4-262">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7e0b4-262">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="7e0b4-263">Proje dosyasında bir veya daha fazla [çalışma zamanı tanımlayıcısı (RID 'ler)](/dotnet/core/rid-catalog)belirtin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-263">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="7e0b4-264">Tek `<RuntimeIdentifier>` bir RID için (tekil) kullanın veya (plural `<RuntimeIdentifiers>` ) kullanarak, noktalı virgülle ayrılmış RID 'ler listesini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-264">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="7e0b4-265">Aşağıdaki örnekte, `win-x86` RID belirtilir:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-265">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="7e0b4-266">Bir komut kabuğundan, uygulamayı, [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutuyla konağın çalışma zamanının sürüm yapılandırmasında yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-266">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="7e0b4-267">Aşağıdaki örnekte, uygulama `win-x86` RID için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-267">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="7e0b4-268">`--runtime` Seçeneğe sağlanan RID, proje dosyasındaki `<RuntimeIdentifier>` (veya `<RuntimeIdentifiers>`) özelliğinde sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-268">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="7e0b4-269">*Bin/Release/{Target Framework}/{RUNTIME Identifier}/Publish* dizininin içeriğini App Service sitesinde siteye taşıyın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-269">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="7e0b4-270">*Klasör içeriğini* yerel sabit sürücünüzden veya ağ paylaşımınızdan, kudu konsolundaki App Service doğrudan sürüklerseniz, dosyaları `D:\home\site\wwwroot` kudu konsolundaki klasöre sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-270">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="7e0b4-271">Protokol ayarları (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="7e0b4-271">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="7e0b4-272">Güvenli Protokol bağlamaları, HTTPS üzerinden isteklere yanıt verme sırasında kullanılacak bir sertifika belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-272">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="7e0b4-273">Bağlama, belirli bir ana bilgisayar adı için verilen geçerli bir özel sertifika ( *. pfx*) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-273">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="7e0b4-274">Daha fazla bilgi için bkz [. Öğretici: Var olan bir özel SSL sertifikasını Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl)bağlayın.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-274">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="7e0b4-275">Web.config’i dönüştürme</span><span class="sxs-lookup"><span data-stu-id="7e0b4-275">Transform web.config</span></span>

<span data-ttu-id="7e0b4-276">*Web. config* 'i yayımlama sırasında dönüştürmeniz gerekiyorsa (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlayın), bkz <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-276">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e0b4-277">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7e0b4-277">Additional resources</span></span>

* [<span data-ttu-id="7e0b4-278">App Service Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="7e0b4-278">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="7e0b4-279">Azure App Service: .NET uygulamalarınızı barındırmak için En Iyi yer (55 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="7e0b4-279">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="7e0b4-280">Azure Cuma: Azure App Service tanılama ve sorun giderme deneyimi (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="7e0b4-280">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="7e0b4-281">Azure App Service tanılamada genel bakış</span><span class="sxs-lookup"><span data-stu-id="7e0b4-281">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="7e0b4-282">Windows Server 'da Azure App Service, [Internet Information Services (IIS)](https://www.iis.net/)kullanır.</span><span class="sxs-lookup"><span data-stu-id="7e0b4-282">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="7e0b4-283">Aşağıdaki konular, temel alınan IIS teknolojisine yöneliktir:</span><span class="sxs-lookup"><span data-stu-id="7e0b4-283">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="7e0b4-284">Windows Server-geçerli ve önceki sürümler için BT Yöneticisi içeriği</span><span class="sxs-lookup"><span data-stu-id="7e0b4-284">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
