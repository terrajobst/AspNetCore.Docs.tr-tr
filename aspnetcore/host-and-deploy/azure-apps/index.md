---
title: Ana bilgisayar ASP.NET Core üzerinde Azure uygulama hizmeti
author: guardrex
description: Yararlı kaynaklara bağlantılarla Azure App Service'te ASP.NET Core uygulamaları barındırmak nasıl bulur.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 4cf81a3e269500a5108f280348fbddd172df10a0
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687509"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="74332-103">Ana bilgisayar ASP.NET Core üzerinde Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="74332-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="74332-104">[Azure uygulama hizmeti](https://azure.microsoft.com/services/app-service/) olan bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) web uygulamalarını barındırmak için ASP.NET Core de dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="74332-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="74332-105">Kullanışlı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="74332-105">Useful resources</span></span>

<span data-ttu-id="74332-106">Azure [Web Apps belge](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynakları için giriş değil.</span><span class="sxs-lookup"><span data-stu-id="74332-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="74332-107">ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="74332-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="74332-108">Hızlı Başlangıç: bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="74332-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="74332-109">Visual Studio oluşturmak ve Windows Azure App Service ASP.NET Core web uygulama dağıtmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="74332-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="74332-110">Hızlı Başlangıç: Linux'ta App Service'te bir .NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="74332-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="74332-111">Oluşturma ve Linux Azure App Service ASP.NET Core web uygulama dağıtmak için komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="74332-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="74332-112">Aşağıdaki makaleler ASP.NET Core belgelerinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="74332-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="74332-113">Visual Studio ile Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="74332-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="74332-114">Visual Studio kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.</span><span class="sxs-lookup"><span data-stu-id="74332-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="74332-115">CLI araçları ile Azure’a yayımlama</span><span class="sxs-lookup"><span data-stu-id="74332-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="74332-116">Git komut satırı istemcisi kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.</span><span class="sxs-lookup"><span data-stu-id="74332-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="74332-117">Visual Studio ve Git ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="74332-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="74332-118">Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="74332-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="74332-119">VSTS ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="74332-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="74332-120">Bir ASP.NET Core uygulama için bir CI yapı ayarlama sonra Azure App Service için sürekli dağıtım sürüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="74332-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="74332-121">Azure Web uygulaması korumalı alan</span><span class="sxs-lookup"><span data-stu-id="74332-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="74332-122">Azure uygulama hizmeti çalışma zamanı yürütme sınırlamaları Azure uygulamaları platformu tarafından zorlanan bulur.</span><span class="sxs-lookup"><span data-stu-id="74332-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="74332-123">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="74332-123">Application configuration</span></span>

<span data-ttu-id="74332-124">ASP.NET Core 2.0 ve daha sonra içindeki üç paketleri [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="74332-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="74332-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="74332-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="74332-126">Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.</span><span class="sxs-lookup"><span data-stu-id="74332-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="74332-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="74332-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="74332-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) Günlükçü uygulamaları Azure App Service tanılama günlüklerini ve günlük özellikleri akış desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="74332-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="74332-129">Proxy sunucusu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="74332-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="74332-130">İletilen üstbilgileri ara yazılımı ve ASP.NET Core Modülü'nü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletmek için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="74332-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="74332-131">Ek proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="74332-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="74332-132">Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="74332-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="74332-133">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="74332-133">Monitoring and logging</span></span>

<span data-ttu-id="74332-134">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="74332-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="74332-135">Nasıl yapılır: Azure uygulama hizmetinde uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="74332-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="74332-136">Kotalar ve uygulamalar ve uygulama hizmeti planları için ölçümlerini gözden öğrenin.</span><span class="sxs-lookup"><span data-stu-id="74332-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="74332-137">Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="74332-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="74332-138">Etkinleştirme ve HTTP durum kodları, başarısız olan istekler ve web sunucusu etkinliğini tanılama günlük erişim bulur.</span><span class="sxs-lookup"><span data-stu-id="74332-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="74332-139">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="74332-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="74332-140">ASP.NET Core uygulamaları hataları işlemek için ortak appoaches anlayın.</span><span class="sxs-lookup"><span data-stu-id="74332-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="74332-141">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="74332-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="74332-142">Azure uygulama hizmeti dağıtımlar ASP.NET Core uygulamaları ile ilgili sorunları tanılamak öğrenin.</span><span class="sxs-lookup"><span data-stu-id="74332-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="74332-143">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="74332-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="74332-144">Sorun giderme önerileri ile Azure App Service/IIS tarafından barındırılan uygulamalar için ortak dağıtım yapılandırma hatalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="74332-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="74332-145">Veri koruma anahtarı halkası ve dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="74332-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="74332-146">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="74332-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="74332-147">Bu klasör, ağ depolama tarafından yedeklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="74332-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="74332-148">Anahtarları bekleyen korumalı değil.</span><span class="sxs-lookup"><span data-stu-id="74332-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="74332-149">Bu klasör, tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri anahtar halkası sağlar.</span><span class="sxs-lookup"><span data-stu-id="74332-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="74332-150">Hazırlama ve üretim gibi ayrı bir dağıtım yuvası anahtar halkası paylaşmayın.</span><span class="sxs-lookup"><span data-stu-id="74332-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="74332-151">Dağıtım yuvaları arasında takası zaman veri koruması kullanan tüm sistemler anahtar halkası önceki yuvasına kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="74332-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="74332-152">ASP.NET tanımlama bilgisi ara veri koruma, tanımlama bilgilerini korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="74332-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="74332-153">Bu standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu imzalanması kullanıcılara yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="74332-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="74332-154">Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="74332-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="74332-155">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="74332-155">Azure Blob Storage</span></span>
* <span data-ttu-id="74332-156">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="74332-156">Azure Key Vault</span></span>
* <span data-ttu-id="74332-157">SQL deposu</span><span class="sxs-lookup"><span data-stu-id="74332-157">SQL store</span></span>
* <span data-ttu-id="74332-158">Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="74332-158">Redis cache</span></span>

<span data-ttu-id="74332-159">Daha fazla bilgi için bkz: [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="74332-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="74332-160">ASP.NET Core Önizleme sürümü Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="74332-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="74332-161">ASP.NET Core Önizleme uygulamaları için Azure App Service ile aşağıdaki yaklaşımlardan dağıtılabilir:</span><span class="sxs-lookup"><span data-stu-id="74332-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="74332-162">Önizleme sitesi uzantısını yükleyin</span><span class="sxs-lookup"><span data-stu-id="74332-162">Install the preview site extension</span></span>](#install-the-preview-site-extension)
* [<span data-ttu-id="74332-163">Kendi içinde bulunan uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="74332-163">Deploy the app self-contained</span></span>](#deploy-the-app-self-contained)
* [<span data-ttu-id="74332-164">Docker Web Apps ile kapsayıcıları için kullanın.</span><span class="sxs-lookup"><span data-stu-id="74332-164">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="74332-165">Önizleme sitesi uzantısını kullanarak bir sorun oluşursa, sorunu açmak [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="74332-165">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="74332-166">Önizleme sitesi uzantısını yükleyin</span><span class="sxs-lookup"><span data-stu-id="74332-166">Install the preview site extension</span></span>

1. <span data-ttu-id="74332-167">Azure portalından, uygulama hizmeti dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="74332-167">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="74332-168">Web uygulaması seçin.</span><span class="sxs-lookup"><span data-stu-id="74332-168">Select the web app.</span></span>
1. <span data-ttu-id="74332-169">Arama kutusuna "ex" girin veya yönetim bölmeleri listesini aşağı kaydırın **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="74332-169">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="74332-170">Seçin **geliştirme araçları** > **uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="74332-170">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="74332-171">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="74332-171">Select **Add**.</span></span>

   ![Önceki adımları ile azure uygulaması dikey](index/_static/x1.png)

1. <span data-ttu-id="74332-173">Seçin **ASP.NET Core uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="74332-173">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="74332-174">Seçin **Tamam** yasal koşulları kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="74332-174">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="74332-175">Seçin **Tamam** uzantıyı yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="74332-175">Select **OK** to install the extension.</span></span>

<span data-ttu-id="74332-176">Ekleme işlemleri tamamladığınızda, en son .NET Core Önizleme yüklenir.</span><span class="sxs-lookup"><span data-stu-id="74332-176">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="74332-177">Çalıştırarak yüklemeyi doğrulayın `dotnet --info` konsolunda.</span><span class="sxs-lookup"><span data-stu-id="74332-177">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="74332-178">Gelen **uygulama hizmeti** dikey penceresinde:</span><span class="sxs-lookup"><span data-stu-id="74332-178">From the **App Service** blade:</span></span>

1. <span data-ttu-id="74332-179">"Arama kutusu veya kaydırma yönetim bölmeleri listede aşağı için con" girin **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="74332-179">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="74332-180">Seçin **geliştirme araçları** > **konsol**.</span><span class="sxs-lookup"><span data-stu-id="74332-180">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="74332-181">Girin `dotnet --info` konsolunda.</span><span class="sxs-lookup"><span data-stu-id="74332-181">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="74332-182">Varsa sürüm `2.1.300-preview1-008174` en son Önizleme sürümlerinden biri çalıştırarak aşağıdaki çıktı elde `dotnet --info` komut isteminde:</span><span class="sxs-lookup"><span data-stu-id="74332-182">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![Önceki adımları ile azure uygulaması dikey](index/_static/cons.png)

<span data-ttu-id="74332-184">Önceki görüntüde gösterildiği ASP.NET Core sürümünü `2.1.300-preview1-008174`, bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="74332-184">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="74332-185">ASP.NET Core en son Önizleme sürümü site uzantısı yapılandırılmış zaman yürüttüğünüzde görünür `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="74332-185">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="74332-186">`dotnet --info` Görüntüler Önizleme yüklendiği site uzantısı yolu.</span><span class="sxs-lookup"><span data-stu-id="74332-186">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="74332-187">Uygulama site uzantısı varsayılan çalıştırılmasının gösterir *ProgramFiles* konumu.</span><span class="sxs-lookup"><span data-stu-id="74332-187">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="74332-188">Görürseniz *ProgramFiles*, siteyi yeniden başlatmak ve Çalıştır `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="74332-188">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="74332-189">**Önizleme site uzantısı ile bir ARM şablonu kullanın**</span><span class="sxs-lookup"><span data-stu-id="74332-189">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="74332-190">ARM şablonunu oluşturmak ve uygulamalarını dağıtmak için kullanılırsa `siteextensions` kaynak türü, bir web uygulaması için site uzantısı eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="74332-190">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="74332-191">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="74332-191">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="74332-192">Kendi içinde bulunan uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="74332-192">Deploy the app self-contained</span></span>

<span data-ttu-id="74332-193">A [kendi içinde bulunan uygulama](/dotnet/core/deploying/#self-contained-deployments-scd) dağıtılabilir, dağıtımda Önizleme çalışma zamanı taşır.</span><span class="sxs-lookup"><span data-stu-id="74332-193">A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment.</span></span> <span data-ttu-id="74332-194">Kendi başına bir uygulama dağıtımı sırasında:</span><span class="sxs-lookup"><span data-stu-id="74332-194">When deploying a self-contained app:</span></span>

* <span data-ttu-id="74332-195">Site hazırlanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="74332-195">The site doesn't need to be prepared.</span></span>
* <span data-ttu-id="74332-196">Uygulama framework bağımlı dağıtım paylaşılan çalışma zamanı ve ana bilgisayar sunucusunda yayımlama, farklı bir biçimde yayımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="74332-196">The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.</span></span>

<span data-ttu-id="74332-197">Tüm ASP.NET Core uygulamaları için bir seçenek müstakil uygulamalardır.</span><span class="sxs-lookup"><span data-stu-id="74332-197">Self-contained apps are an option for all ASP.NET Core apps.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="74332-198">Docker Web Apps ile kapsayıcıları için kullanın.</span><span class="sxs-lookup"><span data-stu-id="74332-198">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="74332-199">[Docker hub'a](https://hub.docker.com/r/microsoft/aspnetcore/) içeren en son Önizleme Docker görüntüler.</span><span class="sxs-lookup"><span data-stu-id="74332-199">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="74332-200">Görüntüleri temel görüntü kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="74332-200">The images can be used as a base image.</span></span> <span data-ttu-id="74332-201">Görüntüyü kullanmak ve Web uygulamaları kapsayıcıları için normal olarak dağıtın.</span><span class="sxs-lookup"><span data-stu-id="74332-201">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74332-202">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="74332-202">Additional resources</span></span>

* [<span data-ttu-id="74332-203">Web Apps'e genel bakış (5 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="74332-203">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="74332-204">Azure uygulama hizmeti: En iyi konağa .NET uygulamalarınızın (55 dakikalık genel bakış videosu) yerleştirin</span><span class="sxs-lookup"><span data-stu-id="74332-204">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="74332-205">Azure Friday: Azure uygulama hizmeti tanılama ve sorun giderme deneyimini (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="74332-205">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="74332-206">Azure uygulama hizmeti tanılama genel bakış</span><span class="sxs-lookup"><span data-stu-id="74332-206">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="74332-207">Windows Server'da Azure uygulama hizmeti kullanan [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="74332-207">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="74332-208">Aşağıdaki konular temel IIS teknoloji ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="74332-208">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="74332-209">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="74332-209">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="74332-210">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="74332-210">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="74332-211">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="74332-211">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="74332-212">ASP.NET Core içeren IIS modülleri</span><span class="sxs-lookup"><span data-stu-id="74332-212">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="74332-213">Microsoft TechNet Kitaplığı: Windows Server</span><span class="sxs-lookup"><span data-stu-id="74332-213">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
