---
title: ASP.NET Core uygulamalarını Azure App Service'e dağıtma
author: guardrex
description: Bu makalede, Azure konak bağlantı içerir ve kaynakları dağıtma.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: b6ff2124aac7e866f630cf359cbd188e88906844
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284701"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="cf6bf-103">ASP.NET Core uygulamalarını Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="cf6bf-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="cf6bf-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) olduğu bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) ASP.NET Core web uygulamalarını barındırmak için dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="cf6bf-105">Faydalı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cf6bf-105">Useful resources</span></span>

<span data-ttu-id="cf6bf-106">Azure [Web Apps belgeleri](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynaklar için platformdur.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="cf6bf-107">ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticiler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="cf6bf-108">Hızlı Başlangıç: Azure'da ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="cf6bf-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="cf6bf-109">Visual Studio, oluşturmak ve Windows üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="cf6bf-110">Hızlı Başlangıç: Linux üzerinde App Service'te .NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="cf6bf-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="cf6bf-111">Oluşturmak ve Linux üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="cf6bf-112">ASP.NET Core belgelerinde aşağıdaki makalelere kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="cf6bf-113">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="cf6bf-114">Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="cf6bf-115">Azure işlem hattı ile ilk işlem hattınızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="cf6bf-115">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="cf6bf-116">ASP.NET Core uygulaması için bir CI derlemesi ayarlayın ve ardından bir Azure App Service'e sürekli dağıtım yayın oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="cf6bf-117">Azure Web uygulama korumalı alanı</span><span class="sxs-lookup"><span data-stu-id="cf6bf-117">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="cf6bf-118">Azure App Service Azure uygulama platformu tarafından zorlanan çalışma zamanı yürütme sınırlamaları keşfedin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="cf6bf-119">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="cf6bf-119">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="cf6bf-120">Platform</span><span class="sxs-lookup"><span data-stu-id="cf6bf-120">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="cf6bf-121">Çalışma zamanları (x64) 64-bit ve 32-bit (x 86) uygulamaları için Azure App Service üzerinde yok.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-121">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="cf6bf-122">[.NET Core SDK'sı](/dotnet/core/sdk) App Service'te 32-bit kullanılabilir, ancak kullanarak 64-bit uygulamaları dağıtabileceğiniz [Kudu](https://github.com/projectkudu/kudu/wiki) konsol veya aracılığıyla [Visual Studio ile MSDeploy yayımlama profilini veya CLI komutunu](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-122">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or via [MSDeploy with a Visual Studio publish profile or CLI command](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="cf6bf-123">Yerel bağımlılıkları olan uygulamalar için çalışma zamanları 32-bit (x 86) uygulamaları için Azure App Service üzerinde yok.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-123">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="cf6bf-124">[.NET Core SDK'sı](/dotnet/core/sdk) 32-bit App Service'teki kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-124">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

### <a name="packages"></a><span data-ttu-id="cf6bf-125">Paketler</span><span class="sxs-lookup"><span data-stu-id="cf6bf-125">Packages</span></span>

<span data-ttu-id="cf6bf-126">Azure App Service'e dağıtılan uygulamalar için otomatik günlük kaydını özellikler sağlamak için aşağıdaki NuGet paketlerini içerir:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-126">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="cf6bf-127">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) kullanan [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration) Azure App Service ile ASP.NET Core açık yukarı tümleştirmesi sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-127">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="cf6bf-128">Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-128">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="cf6bf-129">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-129">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="cf6bf-130">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) Günlükçü uygulamalarını Azure App Service tanılama günlüklerini ve günlük özellikleri akışı desteklemek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-130">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="cf6bf-131">Yukarıdaki paketleri kullanılabilir olmayan [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-131">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="cf6bf-132">.NET Framework veya başvuru hedefleyen uygulamaları `Microsoft.AspNetCore.App` metapackage tek paketlerden uygulamanın proje dosyasında açıkça başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-132">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="cf6bf-133">Azure portalını kullanarak uygulama yapılandırması geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="cf6bf-133">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="cf6bf-134">Azure Portalı'nda uygulama ayarlarını uygulaması için ortam değişkenlerini ayarlamak için izin verir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-134">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="cf6bf-135">Ortam değişkenleri tarafından kullanılabilir [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-135">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="cf6bf-136">Ne zaman bir uygulama ayarı oluşturulduğunda veya Azure Portalı'nda değiştirildiğinde ve **Kaydet** düğmesi seçildiğinde, Azure uygulamasını yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-136">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="cf6bf-137">Hizmet yeniden başlatıldıktan sonra uygulamaya ortam değişkeni kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-137">The environment variable is available to the app after the service restarts.</span></span>

<span data-ttu-id="cf6bf-138">Bir uygulama kullandığında [Web ana bilgisayarı](xref:fundamentals/host/web-host) kullanarak ana bilgisayar oluşturur [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), ana bilgisayar yapılandırma ortam değişkenlerini kullanma `ASPNETCORE_` önek.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-138">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="cf6bf-139">Daha fazla bilgi için <xref:fundamentals/host/web-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-139">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="cf6bf-140">Bir uygulama kullandığında [genel ana bilgisayar](xref:fundamentals/host/generic-host), ortam değişkenleri uygulamanın yapılandırmasını varsayılan olarak yüklü değildir ve geliştirici tarafından yapılandırma sağlayıcısı eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-140">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="cf6bf-141">Yapılandırma sağlayıcısı eklendiğinde Geliştirici ortamı değişken önek belirler.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-141">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="cf6bf-142">Daha fazla bilgi için <xref:fundamentals/host/generic-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-142">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="cf6bf-143">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="cf6bf-143">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="cf6bf-144">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-144">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="cf6bf-145">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-145">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="cf6bf-146">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="cf6bf-147">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="cf6bf-147">Monitoring and logging</span></span>

<span data-ttu-id="cf6bf-148">App Service için otomatik olarak dağıtılan bir ASP.NET Core uygulamaları bir App Service uzantısını alma **ASP.NET Core günlüğü uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-148">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="cf6bf-149">Uzantı Azure günlük kaydını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-149">The extension enables Azure logging.</span></span>

<span data-ttu-id="cf6bf-150">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-150">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="cf6bf-151">Nasıl yapılır: Azure App Service'te uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="cf6bf-151">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="cf6bf-152">Kotalar ve uygulamaları ve App Service planları için ölçümleri gözden geçirmeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-152">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="cf6bf-153">Azure App Service'te web apps için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="cf6bf-153">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="cf6bf-154">HTTP durum kodları, başarısız istekler ve web sunucusu etkinliğini için tanılama günlüğüne kaydetme erişimi nasıl etkinleştirileceği keşfedin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-154">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="cf6bf-155">ASP.NET Core uygulamalarında hata işleme için genel yaklaşımları anlayın.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-155">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="cf6bf-156">ASP.NET Core uygulamaları ile Azure App Service dağıtımı ile ilgili sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-156">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="cf6bf-157">Sık karşılaşılan dağıtım yapılandırma hatalarını sorun giderme önerilerine ile Azure App Service/IIS tarafından barındırılan uygulamalar için bkz.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-157">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="cf6bf-158">Veri koruma anahtarı halka ve dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="cf6bf-158">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="cf6bf-159">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-159">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="cf6bf-160">Bu klasör, ağ depolaması tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-160">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="cf6bf-161">Anahtarlar, bekleme sırasında korunmayan.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-161">Keys aren't protected at rest.</span></span> <span data-ttu-id="cf6bf-162">Bu klasör, anahtar halkası tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-162">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="cf6bf-163">Hazırlama ve üretim gibi farklı dağıtım yuvaları, anahtar halkası paylaşmayın.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-163">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="cf6bf-164">Dağıtım yuvası arasında değiştirme, veri korumayı kullanarak herhangi bir sistem önceki yuvasına anahtar halkası kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-164">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="cf6bf-165">ASP.NET tanımlama bilgisi ara yazılım, veri koruma, tanımlama bilgilerini korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-165">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="cf6bf-166">Bu, standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu kapatmadan kullanıcılara yol açar.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-166">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="cf6bf-167">Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-167">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="cf6bf-168">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="cf6bf-168">Azure Blob Storage</span></span>
* <span data-ttu-id="cf6bf-169">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="cf6bf-169">Azure Key Vault</span></span>
* <span data-ttu-id="cf6bf-170">SQL depolama</span><span class="sxs-lookup"><span data-stu-id="cf6bf-170">SQL store</span></span>
* <span data-ttu-id="cf6bf-171">Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="cf6bf-171">Redis cache</span></span>

<span data-ttu-id="cf6bf-172">Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-172">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="cf6bf-173">ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="cf6bf-173">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="cf6bf-174">Aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-174">Use one of the following approaches:</span></span>

* <span data-ttu-id="cf6bf-175">[Önizleme site uzantısını yüklemek](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-175">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="cf6bf-176">[Kendi içinde uygulama dağıtmak](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-176">[Deploy the app self-contained](#deploy-the-app-self-contained).</span></span>
* <span data-ttu-id="cf6bf-177">[Docker, kapsayıcılar için Web Apps ile kullanma](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-177">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="cf6bf-178">Önizleme sitesi uzantısını yükle</span><span class="sxs-lookup"><span data-stu-id="cf6bf-178">Install the preview site extension</span></span>

<span data-ttu-id="cf6bf-179">Önizleme site uzantısı kullanarak bir sorun ortaya çıkarsa, bir sorun açın [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-179">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="cf6bf-180">Azure Portalı'ndan uygulama hizmetine gidin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-180">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="cf6bf-181">Web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-181">Select the web app.</span></span>
1. <span data-ttu-id="cf6bf-182">Tür "ör" veya "Uzantıları" kaydırma için filtrelemek için arama kutusuna Yönetim Araçları listesini aşağı.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-182">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="cf6bf-183">Seçin **uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-183">Select **Extensions**.</span></span>
1. <span data-ttu-id="cf6bf-184">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-184">Select **Add**.</span></span>
1. <span data-ttu-id="cf6bf-185">Seçin **ASP.NET Core {X.Y} ({x64 | x86}) çalışma zamanı** uzantısı listeden burada `{X.Y}` ASP.NET Core Önizleme sürümüdür ve `{x64|x86}` platformunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-185">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="cf6bf-186">Seçin **Tamam** yasal koşulları kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-186">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="cf6bf-187">Seçin **Tamam** uzantıyı yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-187">Select **OK** to install the extension.</span></span>

<span data-ttu-id="cf6bf-188">İşlem tamamlandığında, en son .NET Core Önizleme yüklenir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-188">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="cf6bf-189">Yüklemeyi doğrulama:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-189">Verify the installation:</span></span>

1. <span data-ttu-id="cf6bf-190">Seçin **Gelişmiş Araçlar**.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-190">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="cf6bf-191">Seçin **Git** içinde **Gelişmiş Araçlar**.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-191">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="cf6bf-192">Seçin **hata ayıklama konsoluna** > **PowerShell** menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-192">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="cf6bf-193">PowerShell komut isteminde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-193">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="cf6bf-194">ASP.NET Core çalışma zamanı sürümü yerine `{X.Y}` ve platformu `{PLATFORM}` komutta:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-194">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```
   <span data-ttu-id="cf6bf-195">Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-195">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="cf6bf-196">Uygulama Hizmetleri uygulama platformu mimarisi (x86/x64), uygulama ayarlarında, bir A serisi işlem üzerinde barındırılan ya da daha iyi katmanını barındıran uygulamalar için Azure Portalı'nda ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-196">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="cf6bf-197">Uygulama, işlem içi modunda çalıştırıldığında ve platform mimarisi için 64-bit (x64) yapılandırılmışsa, ASP.NET Core modülü varsa, 64-bit önizlemesi çalışma zamanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-197">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="cf6bf-198">Yükleme **ASP.NET Core {X.Y} (x64) çalışma zamanı** uzantısı.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-198">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="cf6bf-199">X64 yükledikten sonra çalışma zamanı Önizleme, yüklemeyi doğrulamak için Kudu PowerShell komut penceresinde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-199">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="cf6bf-200">ASP.NET Core çalışma zamanı sürümü yerine `{X.Y}` komutta:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-200">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
> <span data-ttu-id="cf6bf-201">Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-201">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="cf6bf-202">**ASP.NET Core uzantıları** Azure günlük kaydı etkinleştirme gibi ek işlevler üzerinde Azure App Services, ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-202">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="cf6bf-203">Uzantı, Visual Studio'dan dağıtım yaparken otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-203">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="cf6bf-204">Uzantı yüklü değilse, uygulamayı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-204">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="cf6bf-205">**Bir ARM şablonu ile Önizleme site uzantısı kullanma**</span><span class="sxs-lookup"><span data-stu-id="cf6bf-205">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="cf6bf-206">Bir ARM şablonu, uygulamaları oluşturup dağıtmak için kullanılıyorsa `siteextensions` kaynak türü, bir web uygulamasına site uzantısı eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-206">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="cf6bf-207">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-207">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="cf6bf-208">Kendi içinde uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="cf6bf-208">Deploy the app self-contained</span></span>

<span data-ttu-id="cf6bf-209">A [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) önizlemesini hedefleyen çalışma zamanı dağıtımda önizlemesi çalışma zamanı taşır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-209">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="cf6bf-210">Kendi içinde uygulama dağıtırken:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-210">When deploying a self-contained app:</span></span>

* <span data-ttu-id="cf6bf-211">Azure App Service'te site gerektirmeyen [Önizleme site uzantısı](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-211">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="cf6bf-212">Uygulama yayımlama olduğunda daha farklı bir yaklaşım izleyerek yayımlanmalıdır bir [framework bağımlı dağıtım (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-212">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

#### <a name="publish-from-visual-studio"></a><span data-ttu-id="cf6bf-213">Visual Studio'dan yayımlama</span><span class="sxs-lookup"><span data-stu-id="cf6bf-213">Publish from Visual Studio</span></span>

1. <span data-ttu-id="cf6bf-214">Seçin **derleme** > **yayımlama {uygulama-adı}** Visual Studio araç çubuğundan.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-214">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar.</span></span>
1. <span data-ttu-id="cf6bf-215">İçinde **yayımlama hedefi seçin** iletişim kutusunda onaylayın **App Service** seçilir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-215">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="cf6bf-216">**Gelişmiş**'i seçin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-216">Select **Advanced**.</span></span> <span data-ttu-id="cf6bf-217">**Yayımla** iletişim kutusu açılır.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-217">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="cf6bf-218">İçinde **Yayımla** iletişim:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-218">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="cf6bf-219">Onaylayın **yayın** yapılandırması seçili.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-219">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="cf6bf-220">Açık **dağıtım modu** aşağı açılan listesinden **müstakil**.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-220">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="cf6bf-221">Hedef çalışma zamanını şuradan seçin **hedef çalışma zamanı** aşağı açılan listesi.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-221">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="cf6bf-222">Varsayılan, `win-x86` değeridir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-222">The default is `win-x86`.</span></span>
   * <span data-ttu-id="cf6bf-223">Ek dosyaları dağıtım kaldırmanız gerekirse, açık **dosya yayımlama seçeneği** ve hedefteki ek dosyaları kaldırmak için bu onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-223">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="cf6bf-224">**Kaydet**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-224">Select **Save**.</span></span>
1. <span data-ttu-id="cf6bf-225">Yeni bir site veya mevcut bir site Yayımlama Sihirbazı'nın kalan istemleri izleyerek güncelleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-225">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

#### <a name="publish-using-command-line-interface-cli-tools"></a><span data-ttu-id="cf6bf-226">Komut satırı arabirimi (CLI) araçlarını kullanarak yayımla</span><span class="sxs-lookup"><span data-stu-id="cf6bf-226">Publish using command-line interface (CLI) tools</span></span>

1. <span data-ttu-id="cf6bf-227">Proje dosyasında bir veya daha fazla belirtin [çalışma zamanı tanımlayıcılarının (RID'ler)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-227">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="cf6bf-228">Kullanma `<RuntimeIdentifier>` tek RID veya kullanın (tekil) `<RuntimeIdentifiers>` RID'ler noktalı virgülle ayrılmış bir listesini sağlamak üzere (çoğul).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-228">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="cf6bf-229">Aşağıdaki örnekte, `win-x86` RID belirtilir:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-229">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```
1. <span data-ttu-id="cf6bf-230">Bir komut kabuğu'ndan, sürüm yapılandırmasında ana bilgisayarın çalışma zamanı ile uygulamayı yayımlama [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-230">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="cf6bf-231">Aşağıdaki örnekte, uygulama için yayımlanan `win-x86` RID.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-231">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="cf6bf-232">Sağlanan RID `--runtime` seçeneği sağlanmalıdır `<RuntimeIdentifier>` (veya `<RuntimeIdentifiers>`) proje dosyasındaki özellik.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-232">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```
1. <span data-ttu-id="cf6bf-233">İçeriği Taşı *bin/bırakma / {TARGET FRAMEWORK} / {çalışma zamanı TANIMLAYICISI} / publish* App Service'te bir siteye dizin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-233">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="cf6bf-234">Docker, kapsayıcılar için Web Apps ile kullanma</span><span class="sxs-lookup"><span data-stu-id="cf6bf-234">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="cf6bf-235">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) en son içeren Docker görüntüleri önizleme.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-235">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="cf6bf-236">Görüntü, temel görüntü olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-236">The images can be used as a base image.</span></span> <span data-ttu-id="cf6bf-237">Görüntüyü kullanın ve kapsayıcılar için Web Apps için normal olarak dağıtın.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-237">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="protocol-settings-https"></a><span data-ttu-id="cf6bf-238">Protokol ayarları (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="cf6bf-238">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="cf6bf-239">Güvenli protokol bağlamalar HTTPS üzerinden isteklerine yanıt verirken kullanmak üzere bir sertifika belirtin izin verir.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-239">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="cf6bf-240">Bağlama, geçerli özel sertifikaları gerektirir (*.pfx*) belirli ana bilgisayar adı için verilmiş.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-240">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="cf6bf-241">Daha fazla bilgi için [Öğreticisi: Azure Web Apps'e mevcut özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-241">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf6bf-242">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cf6bf-242">Additional resources</span></span>

* [<span data-ttu-id="cf6bf-243">Web Apps'e genel bakış (5 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="cf6bf-243">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="cf6bf-244">Azure uygulama hizmeti: En iyi .NET uygulamalarınızı (55 dakikalık genel bakış videosu) konağa yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="cf6bf-244">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="cf6bf-245">Azure Friday: Azure App Service tanılama ve sorun giderme deneyimini (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="cf6bf-245">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="cf6bf-246">Azure App Service tanılama genel bakış</span><span class="sxs-lookup"><span data-stu-id="cf6bf-246">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="cf6bf-247">Windows Server üzerinde Azure App Service kullanan [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="cf6bf-247">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="cf6bf-248">Aşağıdaki konular temel alınan IIS teknolojiye ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="cf6bf-248">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="cf6bf-249">Microsoft TechNet Kitaplığı: Windows Server</span><span class="sxs-lookup"><span data-stu-id="cf6bf-249">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
