---
title: "Ana bilgisayar ASP.NET Core üzerinde Azure uygulama hizmeti"
author: guardrex
description: "Yararlı kaynaklara bağlantılarla Azure App Service'te ASP.NET Core uygulamaları barındırmak nasıl bulur."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: cefbc27c8091a2ed1441663e3779d67aae2c64dd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="fe6e6-103">Ana bilgisayar ASP.NET Core üzerinde Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="fe6e6-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="fe6e6-104">[Azure uygulama hizmeti](https://azure.microsoft.com/services/app-service/) olan bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) web uygulamalarını barındırmak için ASP.NET Core de dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

## <a name="useful-resources"></a><span data-ttu-id="fe6e6-105">Kullanışlı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fe6e6-105">Useful resources</span></span>

<span data-ttu-id="fe6e6-106">Azure [Web Apps belge](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynakları için giriş değil.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="fe6e6-107">ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="fe6e6-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="fe6e6-108">Hızlı Başlangıç: bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="fe6e6-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="fe6e6-109">Visual Studio oluşturmak ve Windows Azure App Service ASP.NET Core web uygulama dağıtmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="fe6e6-110">Hızlı Başlangıç: Linux'ta App Service'te bir .NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="fe6e6-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="fe6e6-111">Oluşturma ve Linux Azure App Service ASP.NET Core web uygulama dağıtmak için komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="fe6e6-112">Aşağıdaki makaleler ASP.NET Core belgelerinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fe6e6-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="fe6e6-113">Visual Studio ile Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe6e6-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="fe6e6-114">Visual Studio kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="fe6e6-115">CLI araçları ile Azure’a yayımlama</span><span class="sxs-lookup"><span data-stu-id="fe6e6-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="fe6e6-116">Git komut satırı istemcisi kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="fe6e6-117">Visual Studio ve Git ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="fe6e6-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="fe6e6-118">Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="fe6e6-119">VSTS ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="fe6e6-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="fe6e6-120">Bir ASP.NET Core uygulama için bir CI yapı ayarlama sonra Azure App Service için sürekli dağıtım sürüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="fe6e6-121">Azure Web uygulaması korumalı alan</span><span class="sxs-lookup"><span data-stu-id="fe6e6-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="fe6e6-122">Azure uygulama hizmeti çalışma zamanı yürütme sınırlamaları Azure uygulamaları platformu tarafından zorlanan bulur.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="fe6e6-123">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fe6e6-123">Application configuration</span></span>

<span data-ttu-id="fe6e6-124">ASP.NET Core 2.0 ve daha sonra içindeki üç paketleri [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="fe6e6-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="fe6e6-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="fe6e6-126">Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="fe6e6-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="fe6e6-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="fe6e6-129">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="fe6e6-129">Monitoring and logging</span></span>

<span data-ttu-id="fe6e6-130">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="fe6e6-130">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="fe6e6-131">Nasıl yapılır: Azure uygulama hizmetinde uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="fe6e6-131">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="fe6e6-132">Kotalar ve uygulamalar ve uygulama hizmeti planları için ölçümlerini gözden öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-132">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="fe6e6-133">Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="fe6e6-133">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="fe6e6-134">Etkinleştirme ve HTTP durum kodları, başarısız olan istekler ve web sunucusu etkinliğini tanılama günlük erişim bulur.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-134">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="fe6e6-135">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="fe6e6-135">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="fe6e6-136">ASP.NET Core uygulamaları hataları işlemek için ortak appoaches anlayın.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-136">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="fe6e6-137">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="fe6e6-137">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="fe6e6-138">Azure uygulama hizmeti dağıtımlar ASP.NET Core uygulamaları ile ilgili sorunları tanılamak öğrenin.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-138">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="fe6e6-139">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="fe6e6-139">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="fe6e6-140">Sorun giderme önerileri ile Azure App Service/IIS tarafından barındırılan uygulamalar için ortak dağıtım yapılandırma hatalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-140">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="fe6e6-141">Veri koruma anahtarı halkası ve dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="fe6e6-141">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="fe6e6-142">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-142">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="fe6e6-143">Bu klasör, ağ depolama tarafından yedeklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-143">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="fe6e6-144">Anahtarları bekleyen korumalı değil.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-144">Keys aren't protected at rest.</span></span> <span data-ttu-id="fe6e6-145">Bu klasör, tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri anahtar halkası sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-145">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="fe6e6-146">Hazırlama ve üretim gibi ayrı bir dağıtım yuvası anahtar halkası paylaşmayın.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-146">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="fe6e6-147">Dağıtım yuvaları arasında takası zaman veri koruması kullanan tüm sistemler anahtar halkası önceki yuvasına kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-147">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="fe6e6-148">ASP.NET tanımlama bilgisi ara veri koruma, tanımlama bilgilerini korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-148">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="fe6e6-149">Bu standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu imzalanması kullanıcılara yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe6e6-149">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="fe6e6-150">Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="fe6e6-150">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="fe6e6-151">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="fe6e6-151">Azure Blob Storage</span></span>
* <span data-ttu-id="fe6e6-152">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="fe6e6-152">Azure Key Vault</span></span>
* <span data-ttu-id="fe6e6-153">SQL deposu</span><span class="sxs-lookup"><span data-stu-id="fe6e6-153">SQL store</span></span>
* <span data-ttu-id="fe6e6-154">Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="fe6e6-154">Redis cache</span></span>

<span data-ttu-id="fe6e6-155">Daha fazla bilgi için bkz: [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="fe6e6-155">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe6e6-156">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fe6e6-156">Additional resources</span></span>

* [<span data-ttu-id="fe6e6-157">Web Apps'e genel bakış (5 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="fe6e6-157">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="fe6e6-158">Azure uygulama hizmeti: En iyi konağa .NET uygulamalarınızın (55 dakikalık genel bakış videosu) yerleştirin</span><span class="sxs-lookup"><span data-stu-id="fe6e6-158">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="fe6e6-159">Azure Friday: Azure uygulama hizmeti tanılama ve sorun giderme deneyimini (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="fe6e6-159">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="fe6e6-160">Azure uygulama hizmeti tanılama genel bakış</span><span class="sxs-lookup"><span data-stu-id="fe6e6-160">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="fe6e6-161">Windows Server'da Azure uygulama hizmeti kullanan [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="fe6e6-161">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="fe6e6-162">Aşağıdaki konular temel IIS teknoloji ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="fe6e6-162">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="fe6e6-163">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="fe6e6-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="fe6e6-164">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="fe6e6-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="fe6e6-165">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="fe6e6-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="fe6e6-166">ASP.NET çekirdeği ile IIS modüllerini kullanma</span><span class="sxs-lookup"><span data-stu-id="fe6e6-166">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="fe6e6-167">Microsoft TechNet Kitaplığı: Windows Server</span><span class="sxs-lookup"><span data-stu-id="fe6e6-167">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
