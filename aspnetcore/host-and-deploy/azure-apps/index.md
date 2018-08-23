---
title: Azure App Service'te ASP.NET Core barındırma
author: guardrex
description: Faydalı kaynaklara bağlantılarla Azure App Service'te ASP.NET Core uygulamaları barındırmak nasıl keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 9a7d20378cac597b748d8a60eb0f0bf17c9ba082
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41756288"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="da5a6-103">Azure App Service'te ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="da5a6-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="da5a6-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) olduğu bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) ASP.NET Core web uygulamalarını barındırmak için dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="da5a6-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="da5a6-105">Faydalı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="da5a6-105">Useful resources</span></span>

<span data-ttu-id="da5a6-106">Azure [Web Apps belgeleri](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynaklar için platformdur.</span><span class="sxs-lookup"><span data-stu-id="da5a6-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="da5a6-107">ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticiler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="da5a6-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="da5a6-108">Hızlı Başlangıç: Azure'da bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="da5a6-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="da5a6-109">Visual Studio, oluşturmak ve Windows üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="da5a6-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="da5a6-110">Hızlı Başlangıç: Linux üzerinde App Service'te .NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="da5a6-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="da5a6-111">Oluşturmak ve Linux üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="da5a6-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="da5a6-112">ASP.NET Core belgelerinde aşağıdaki makalelere kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="da5a6-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="da5a6-113">Visual Studio ile Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="da5a6-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="da5a6-114">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="da5a6-115">CLI araçları ile Azure’a yayımlama</span><span class="sxs-lookup"><span data-stu-id="da5a6-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="da5a6-116">Azure App Service'e Git komut satırı istemcisini kullanarak bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="da5a6-117">Visual Studio ve Git ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="da5a6-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="da5a6-118">Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="da5a6-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="da5a6-119">VSTS ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="da5a6-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="da5a6-120">ASP.NET Core uygulaması için bir CI derlemesi ayarlayın ve ardından bir Azure App Service'e sürekli dağıtım yayın oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da5a6-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="da5a6-121">Azure Web uygulama korumalı alanı</span><span class="sxs-lookup"><span data-stu-id="da5a6-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="da5a6-122">Azure App Service Azure uygulama platformu tarafından zorlanan çalışma zamanı yürütme sınırlamaları keşfedin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="da5a6-123">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="da5a6-123">Application configuration</span></span>

<span data-ttu-id="da5a6-124">ASP.NET Core 2.0 veya sonraki sürümlerde, aşağıdaki NuGet paketlerini Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="da5a6-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="da5a6-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) kullanan [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration) Azure App Service ile ASP.NET Core açık yukarı tümleştirmesi sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="da5a6-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="da5a6-126">Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.</span><span class="sxs-lookup"><span data-stu-id="da5a6-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="da5a6-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="da5a6-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="da5a6-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) Günlükçü uygulamalarını Azure App Service tanılama günlüklerini ve günlük özellikleri akışı desteklemek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="da5a6-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="da5a6-129">.NET Core'u hedefleyen ve bunlara başvurma [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), paketleri zaten dahildir.</span><span class="sxs-lookup"><span data-stu-id="da5a6-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="da5a6-130">Eksik paketleri yeni gelen [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="da5a6-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="da5a6-131">.NET Framework'ü hedefleyen veya başvuru `Microsoft.AspNetCore.App` metapackage, tek tek günlük paketleri başvuru.</span><span class="sxs-lookup"><span data-stu-id="da5a6-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="da5a6-132">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="da5a6-132">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="da5a6-133">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="da5a6-133">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="da5a6-134">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="da5a6-134">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="da5a6-135">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="da5a6-135">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="da5a6-136">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="da5a6-136">Monitoring and logging</span></span>

<span data-ttu-id="da5a6-137">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="da5a6-137">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="da5a6-138">Nasıl yapılır: Azure App Service'te uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="da5a6-138">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="da5a6-139">Kotalar ve uygulamaları ve App Service planları için ölçümleri gözden geçirmeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-139">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="da5a6-140">Azure App Service'te web apps için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="da5a6-140">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="da5a6-141">HTTP durum kodları, başarısız istekler ve web sunucusu etkinliğini için tanılama günlüğüne kaydetme erişimi nasıl etkinleştirileceği keşfedin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-141">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="da5a6-142">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="da5a6-142">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="da5a6-143">ASP.NET Core uygulamalarında hata işleme için genel yaklaşımları anlayın.</span><span class="sxs-lookup"><span data-stu-id="da5a6-143">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="da5a6-144">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="da5a6-144">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="da5a6-145">ASP.NET Core uygulamaları ile Azure App Service dağıtımı ile ilgili sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-145">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="da5a6-146">Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="da5a6-146">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="da5a6-147">Sık karşılaşılan dağıtım yapılandırma hatalarını sorun giderme önerilerine ile Azure App Service/IIS tarafından barındırılan uygulamalar için bkz.</span><span class="sxs-lookup"><span data-stu-id="da5a6-147">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="da5a6-148">Veri koruma anahtarı halka ve dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="da5a6-148">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="da5a6-149">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="da5a6-149">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="da5a6-150">Bu klasör, ağ depolaması tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="da5a6-150">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="da5a6-151">Anahtarlar, bekleme sırasında korunmayan.</span><span class="sxs-lookup"><span data-stu-id="da5a6-151">Keys aren't protected at rest.</span></span> <span data-ttu-id="da5a6-152">Bu klasör, anahtar halkası tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="da5a6-152">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="da5a6-153">Hazırlama ve üretim gibi farklı dağıtım yuvaları, anahtar halkası paylaşmayın.</span><span class="sxs-lookup"><span data-stu-id="da5a6-153">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="da5a6-154">Dağıtım yuvası arasında değiştirme, veri korumayı kullanarak herhangi bir sistem önceki yuvasına anahtar halkası kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="da5a6-154">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="da5a6-155">ASP.NET tanımlama bilgisi ara yazılım, veri koruma, tanımlama bilgilerini korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="da5a6-155">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="da5a6-156">Bu, standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu kapatmadan kullanıcılara yol açar.</span><span class="sxs-lookup"><span data-stu-id="da5a6-156">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="da5a6-157">Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="da5a6-157">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="da5a6-158">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="da5a6-158">Azure Blob Storage</span></span>
* <span data-ttu-id="da5a6-159">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="da5a6-159">Azure Key Vault</span></span>
* <span data-ttu-id="da5a6-160">SQL depolama</span><span class="sxs-lookup"><span data-stu-id="da5a6-160">SQL store</span></span>
* <span data-ttu-id="da5a6-161">Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="da5a6-161">Redis cache</span></span>

<span data-ttu-id="da5a6-162">Daha fazla bilgi için [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="da5a6-162">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="da5a6-163">ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="da5a6-163">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="da5a6-164">ASP.NET Core Önizleme uygulamalarını Azure App Service için aşağıdaki yaklaşımlardan ile dağıtılabilir:</span><span class="sxs-lookup"><span data-stu-id="da5a6-164">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="da5a6-165">[Önizleme sitesi uzantısını yükle](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="da5a6-165">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="da5a6-166">Docker, kapsayıcılar için Web Apps ile kullanma</span><span class="sxs-lookup"><span data-stu-id="da5a6-166">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="da5a6-167">Önizleme site uzantısı kullanarak bir sorun ortaya çıkarsa, bir sorun açın [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="da5a6-167">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="da5a6-168">Önizleme sitesi uzantısını yükle</span><span class="sxs-lookup"><span data-stu-id="da5a6-168">Install the preview site extension</span></span>

1. <span data-ttu-id="da5a6-169">Azure portalda, App Service dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-169">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="da5a6-170">Web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-170">Select the web app.</span></span>
1. <span data-ttu-id="da5a6-171">Arama kutusuna "ör" girin veya yönetim bölmelerini listesini aşağı kaydırın **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="da5a6-171">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="da5a6-172">Seçin **geliştirme araçları** > **uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="da5a6-172">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="da5a6-173">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="da5a6-173">Select **Add**.</span></span>

   ![Önceki adımları ile azure uygulama dikey penceresi](index/_static/x1.png)

1. <span data-ttu-id="da5a6-175">Seçin **ASP.NET Core uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="da5a6-175">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="da5a6-176">Seçin **Tamam** yasal koşulları kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="da5a6-176">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="da5a6-177">Seçin **Tamam** uzantıyı yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="da5a6-177">Select **OK** to install the extension.</span></span>

<span data-ttu-id="da5a6-178">Ekleme işlemleri tamamladığınızda, en son .NET Core Önizleme yüklenir.</span><span class="sxs-lookup"><span data-stu-id="da5a6-178">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="da5a6-179">Çalıştırarak yüklemeyi doğrulayın `dotnet --info` konsolunda.</span><span class="sxs-lookup"><span data-stu-id="da5a6-179">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="da5a6-180">Gelen **App Service** dikey penceresinde:</span><span class="sxs-lookup"><span data-stu-id="da5a6-180">From the **App Service** blade:</span></span>

1. <span data-ttu-id="da5a6-181">"Arama kutusu veya kaydırma yönetim bölmeleri serilerini için con" girin **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="da5a6-181">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="da5a6-182">Seçin **geliştirme araçları** > **konsol**.</span><span class="sxs-lookup"><span data-stu-id="da5a6-182">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="da5a6-183">Girin `dotnet --info` konsolunda.</span><span class="sxs-lookup"><span data-stu-id="da5a6-183">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="da5a6-184">Varsa sürüm `2.1.300-preview1-008174` en son Önizleme sürümü olduğundan aşağıdaki çıktıyı çalıştırarak elde edilen `dotnet --info` komut isteminde:</span><span class="sxs-lookup"><span data-stu-id="da5a6-184">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![Önceki adımları ile azure uygulama dikey penceresi](index/_static/cons.png)

<span data-ttu-id="da5a6-186">ASP.NET Core önceki görüntüde gösterilen sürümünü `2.1.300-preview1-008174`, bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="da5a6-186">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="da5a6-187">ASP.NET Core site uzantısını yapılandırılmış zaman en son önizleme sürümünü yürüttüğünüzde görünür `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="da5a6-187">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="da5a6-188">`dotnet --info` Önizleme burada yüklenmiş site uzantısı yolunu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="da5a6-188">The `dotnet --info` displays the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="da5a6-189">Uygulama yerine varsayılan site uzantısını çalıştırılmasının gösterir *ProgramFiles* konumu.</span><span class="sxs-lookup"><span data-stu-id="da5a6-189">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="da5a6-190">Görürseniz *ProgramFiles*, siteyi yeniden başlatın ve çalıştırın `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="da5a6-190">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="da5a6-191">**Bir ARM şablonu ile Önizleme site uzantısı kullanma**</span><span class="sxs-lookup"><span data-stu-id="da5a6-191">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="da5a6-192">Bir ARM şablonu, uygulamaları oluşturup dağıtmak için kullanılıyorsa `siteextensions` kaynak türü, bir web uygulamasına site uzantısı eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="da5a6-192">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="da5a6-193">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da5a6-193">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="da5a6-194">Docker, kapsayıcılar için Web Apps ile kullanma</span><span class="sxs-lookup"><span data-stu-id="da5a6-194">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="da5a6-195">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) en son içeren Docker görüntüleri önizleme.</span><span class="sxs-lookup"><span data-stu-id="da5a6-195">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="da5a6-196">Görüntü, temel görüntü olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="da5a6-196">The images can be used as a base image.</span></span> <span data-ttu-id="da5a6-197">Görüntüyü kullanın ve kapsayıcılar için Web Apps için normal olarak dağıtın.</span><span class="sxs-lookup"><span data-stu-id="da5a6-197">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da5a6-198">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="da5a6-198">Additional resources</span></span>

* [<span data-ttu-id="da5a6-199">Web Apps'e genel bakış (5 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="da5a6-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="da5a6-200">Azure App Service: Konak için en iyi, .NET uygulamalarınızı (55 dakikalık genel bakış videosu) yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="da5a6-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="da5a6-201">Azure Friday: Azure App Service tanılama ve sorun giderme deneyimini (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="da5a6-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="da5a6-202">Azure App Service tanılama genel bakış</span><span class="sxs-lookup"><span data-stu-id="da5a6-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="da5a6-203">Windows Server üzerinde Azure App Service kullanan [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="da5a6-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="da5a6-204">Aşağıdaki konular temel alınan IIS teknolojiye ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="da5a6-204">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="da5a6-205">Microsoft TechNet Kitaplığı'de: Windows Server</span><span class="sxs-lookup"><span data-stu-id="da5a6-205">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
