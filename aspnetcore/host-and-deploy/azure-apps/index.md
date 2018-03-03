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
ms.openlocfilehash: 8da97b2148c0a784e3b1fdfe210db35e3fcceade
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="07b6c-103">Ana bilgisayar ASP.NET Core üzerinde Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="07b6c-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="07b6c-104">[Azure uygulama hizmeti](https://azure.microsoft.com/services/app-service/) olan bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) web uygulamalarını barındırmak için ASP.NET Core de dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="07b6c-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

## <a name="useful-resources"></a><span data-ttu-id="07b6c-105">Kullanışlı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="07b6c-105">Useful resources</span></span>

<span data-ttu-id="07b6c-106">Azure [Web Apps belge](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynakları için giriş değil.</span><span class="sxs-lookup"><span data-stu-id="07b6c-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="07b6c-107">ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="07b6c-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="07b6c-108">Hızlı Başlangıç: bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="07b6c-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="07b6c-109">Visual Studio oluşturmak ve Windows Azure App Service ASP.NET Core web uygulama dağıtmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="07b6c-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="07b6c-110">Hızlı Başlangıç: Linux'ta App Service'te bir .NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="07b6c-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="07b6c-111">Oluşturma ve Linux Azure App Service ASP.NET Core web uygulama dağıtmak için komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="07b6c-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="07b6c-112">Aşağıdaki makaleler ASP.NET Core belgelerinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="07b6c-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="07b6c-113">Visual Studio ile Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="07b6c-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="07b6c-114">Visual Studio kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.</span><span class="sxs-lookup"><span data-stu-id="07b6c-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="07b6c-115">CLI araçları ile Azure’a yayımlama</span><span class="sxs-lookup"><span data-stu-id="07b6c-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="07b6c-116">Git komut satırı istemcisi kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin.</span><span class="sxs-lookup"><span data-stu-id="07b6c-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="07b6c-117">Visual Studio ve Git ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="07b6c-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="07b6c-118">Visual Studio kullanarak bir ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtımı için Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="07b6c-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="07b6c-119">VSTS ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="07b6c-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="07b6c-120">Bir ASP.NET Core uygulama için bir CI yapı ayarlama sonra Azure App Service için sürekli dağıtım sürüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="07b6c-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="07b6c-121">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="07b6c-121">Application configuration</span></span>

<span data-ttu-id="07b6c-122">ASP.NET Core 2.0 ve daha sonra içindeki üç paketleri [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="07b6c-122">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="07b6c-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="07b6c-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="07b6c-124">Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.</span><span class="sxs-lookup"><span data-stu-id="07b6c-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="07b6c-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="07b6c-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="07b6c-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span><span class="sxs-lookup"><span data-stu-id="07b6c-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="07b6c-127">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="07b6c-127">Monitoring and logging</span></span>

<span data-ttu-id="07b6c-128">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="07b6c-128">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="07b6c-129">Nasıl yapılır: Azure uygulama hizmetinde uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="07b6c-129">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="07b6c-130">Kotalar ve uygulamalar ve uygulama hizmeti planları için ölçümlerini gözden öğrenin.</span><span class="sxs-lookup"><span data-stu-id="07b6c-130">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="07b6c-131">Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="07b6c-131">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="07b6c-132">Etkinleştirme ve HTTP durum kodları, başarısız olan istekler ve web sunucusu etkinliğini tanılama günlük erişim bulur.</span><span class="sxs-lookup"><span data-stu-id="07b6c-132">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="07b6c-133">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="07b6c-133">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="07b6c-134">ASP.NET Core uygulamaları hataları işlemek için ortak appoaches anlayın.</span><span class="sxs-lookup"><span data-stu-id="07b6c-134">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="07b6c-135">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="07b6c-135">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="07b6c-136">Azure uygulama hizmeti dağıtımlar ASP.NET Core uygulamaları ile ilgili sorunları tanılamak öğrenin.</span><span class="sxs-lookup"><span data-stu-id="07b6c-136">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="07b6c-137">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="07b6c-137">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="07b6c-138">Sorun giderme önerileri ile Azure App Service/IIS tarafından barındırılan uygulamalar için ortak dağıtım yapılandırma hatalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="07b6c-138">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="07b6c-139">Veri koruma anahtarı halkası ve dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="07b6c-139">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="07b6c-140">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="07b6c-140">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="07b6c-141">Bu klasör, ağ depolama tarafından yedeklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="07b6c-141">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="07b6c-142">Anahtarları bekleyen korumalı değil.</span><span class="sxs-lookup"><span data-stu-id="07b6c-142">Keys aren't protected at rest.</span></span> <span data-ttu-id="07b6c-143">Bu klasör, tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri anahtar halkası sağlar.</span><span class="sxs-lookup"><span data-stu-id="07b6c-143">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="07b6c-144">Hazırlama ve üretim gibi ayrı bir dağıtım yuvası anahtar halkası paylaşmayın.</span><span class="sxs-lookup"><span data-stu-id="07b6c-144">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="07b6c-145">Dağıtım yuvaları arasında takası zaman veri koruması kullanan tüm sistemler anahtar halkası önceki yuvasına kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="07b6c-145">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="07b6c-146">ASP.NET tanımlama bilgisi ara veri koruma, tanımlama bilgilerini korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="07b6c-146">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="07b6c-147">Bu standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu imzalanması kullanıcılara yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="07b6c-147">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="07b6c-148">Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="07b6c-148">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="07b6c-149">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="07b6c-149">Azure Blob Storage</span></span>
* <span data-ttu-id="07b6c-150">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="07b6c-150">Azure Key Vault</span></span>
* <span data-ttu-id="07b6c-151">SQL deposu</span><span class="sxs-lookup"><span data-stu-id="07b6c-151">SQL store</span></span>
* <span data-ttu-id="07b6c-152">Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="07b6c-152">Redis cache</span></span>

<span data-ttu-id="07b6c-153">Daha fazla bilgi için bkz: [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="07b6c-153">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="07b6c-154">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="07b6c-154">Additional resources</span></span>

* [<span data-ttu-id="07b6c-155">Web Apps'e genel bakış (5 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="07b6c-155">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="07b6c-156">Azure uygulama hizmeti: En iyi konağa .NET uygulamalarınızın (55 dakikalık genel bakış videosu) yerleştirin</span><span class="sxs-lookup"><span data-stu-id="07b6c-156">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="07b6c-157">Azure Friday: Azure uygulama hizmeti tanılama ve sorun giderme deneyimini (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="07b6c-157">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="07b6c-158">Azure uygulama hizmeti tanılama genel bakış</span><span class="sxs-lookup"><span data-stu-id="07b6c-158">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="07b6c-159">Windows Server'da Azure uygulama hizmeti kullanan [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="07b6c-159">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="07b6c-160">Aşağıdaki konular temel IIS teknoloji ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="07b6c-160">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="07b6c-161">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="07b6c-161">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="07b6c-162">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="07b6c-162">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="07b6c-163">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="07b6c-163">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="07b6c-164">ASP.NET çekirdeği ile IIS modüllerini kullanma</span><span class="sxs-lookup"><span data-stu-id="07b6c-164">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="07b6c-165">Microsoft TechNet Kitaplığı: Windows Server</span><span class="sxs-lookup"><span data-stu-id="07b6c-165">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
