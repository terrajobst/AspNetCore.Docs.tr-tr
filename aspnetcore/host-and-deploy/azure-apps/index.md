---
title: ASP.NET Core uygulamalarını Azure App Service'e dağıtma
author: guardrex
description: Bu makalede, Azure konak bağlantı içerir ve kaynakları dağıtma.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 315261c4d20970fc399cc2a879dd452bdf3be93f
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326062"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="19859-103">ASP.NET Core uygulamalarını Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="19859-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="19859-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) olduğu bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) ASP.NET Core web uygulamalarını barındırmak için dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="19859-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="19859-105">Faydalı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="19859-105">Useful resources</span></span>

<span data-ttu-id="19859-106">Azure [Web Apps belgeleri](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynaklar için platformdur.</span><span class="sxs-lookup"><span data-stu-id="19859-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="19859-107">ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticiler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="19859-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="19859-108">Hızlı Başlangıç: Azure'da bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="19859-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="19859-109">Visual Studio, oluşturmak ve Windows üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="19859-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="19859-110">Hızlı Başlangıç: Linux üzerinde App Service'te .NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="19859-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="19859-111">Oluşturmak ve Linux üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="19859-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="19859-112">ASP.NET Core belgelerinde aşağıdaki makalelere kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="19859-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="19859-113">Visual Studio ile Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="19859-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="19859-114">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="19859-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="19859-115">Visual Studio ve Git ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="19859-115">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="19859-116">Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="19859-116">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="19859-117">Azure işlem hattı ile ilk işlem hattınızı oluşturun</span><span class="sxs-lookup"><span data-stu-id="19859-117">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="19859-118">ASP.NET Core uygulaması için bir CI derlemesi ayarlayın ve ardından bir Azure App Service'e sürekli dağıtım yayın oluşturun.</span><span class="sxs-lookup"><span data-stu-id="19859-118">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="19859-119">Azure Web uygulama korumalı alanı</span><span class="sxs-lookup"><span data-stu-id="19859-119">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="19859-120">Azure App Service Azure uygulama platformu tarafından zorlanan çalışma zamanı yürütme sınırlamaları keşfedin.</span><span class="sxs-lookup"><span data-stu-id="19859-120">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="19859-121">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="19859-121">Application configuration</span></span>

<span data-ttu-id="19859-122">ASP.NET Core 2.0 veya sonraki sürümlerde, aşağıdaki NuGet paketlerini Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="19859-122">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="19859-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) kullanan [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration) Azure App Service ile ASP.NET Core açık yukarı tümleştirmesi sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="19859-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="19859-124">Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.</span><span class="sxs-lookup"><span data-stu-id="19859-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="19859-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="19859-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="19859-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) Günlükçü uygulamalarını Azure App Service tanılama günlüklerini ve günlük özellikleri akışı desteklemek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="19859-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="19859-127">.NET Core'u hedefleyen ve bunlara başvurma [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), paketleri zaten dahildir.</span><span class="sxs-lookup"><span data-stu-id="19859-127">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="19859-128">Eksik paketleri yeni gelen [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="19859-128">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="19859-129">.NET Framework'ü hedefleyen veya başvuru `Microsoft.AspNetCore.App` metapackage, tek tek günlük paketleri başvuru.</span><span class="sxs-lookup"><span data-stu-id="19859-129">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="19859-130">Azure portalını kullanarak uygulama yapılandırması geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="19859-130">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="19859-131">**Uygulama ayarları** alanının **uygulama ayarları** dikey penceresinde, uygulama için ortam değişkenlerini ayarlamak için izin verir.</span><span class="sxs-lookup"><span data-stu-id="19859-131">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="19859-132">Ortam değişkenleri tarafından kullanılabilir [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="19859-132">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="19859-133">Ne zaman bir uygulama ayarı oluşturulduğunda veya Azure Portalı'nda değiştirildiğinde ve **Kaydet** düğmesi seçildiğinde, Azure uygulamasını yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="19859-133">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="19859-134">Hizmet yeniden başlatıldıktan sonra uygulamaya ortam değişkeni kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19859-134">The environment variable is available to the app after the service restarts.</span></span>

<span data-ttu-id="19859-135">Bir uygulama kullandığında [Web ana bilgisayarı](xref:fundamentals/host/web-host) kullanarak ana bilgisayar oluşturur [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), ana bilgisayar yapılandırma ortam değişkenlerini kullanma `ASPNETCORE_` önek.</span><span class="sxs-lookup"><span data-stu-id="19859-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="19859-136">Daha fazla bilgi için <xref:fundamentals/host/web-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="19859-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="19859-137">Bir uygulama kullandığında [genel ana bilgisayar](xref:fundamentals/host/generic-host), ortam değişkenleri uygulamanın yapılandırmasını varsayılan olarak yüklü değildir ve geliştirici tarafından yapılandırma sağlayıcısı eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="19859-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="19859-138">Yapılandırma sağlayıcısı eklendiğinde Geliştirici ortamı değişken önek belirler.</span><span class="sxs-lookup"><span data-stu-id="19859-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="19859-139">Daha fazla bilgi için <xref:fundamentals/host/generic-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="19859-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="19859-140">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="19859-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="19859-141">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="19859-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="19859-142">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="19859-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="19859-143">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="19859-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="19859-144">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="19859-144">Monitoring and logging</span></span>

<span data-ttu-id="19859-145">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="19859-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="19859-146">Nasıl yapılır: Azure App Service'te uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="19859-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="19859-147">Kotalar ve uygulamaları ve App Service planları için ölçümleri gözden geçirmeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="19859-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="19859-148">Azure App Service'te web apps için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="19859-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="19859-149">HTTP durum kodları, başarısız istekler ve web sunucusu etkinliğini için tanılama günlüğüne kaydetme erişimi nasıl etkinleştirileceği keşfedin.</span><span class="sxs-lookup"><span data-stu-id="19859-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="19859-150">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="19859-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="19859-151">ASP.NET Core uygulamalarında hata işleme için genel yaklaşımları anlayın.</span><span class="sxs-lookup"><span data-stu-id="19859-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="19859-152">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="19859-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="19859-153">ASP.NET Core uygulamaları ile Azure App Service dağıtımı ile ilgili sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="19859-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="19859-154">Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="19859-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="19859-155">Sık karşılaşılan dağıtım yapılandırma hatalarını sorun giderme önerilerine ile Azure App Service/IIS tarafından barındırılan uygulamalar için bkz.</span><span class="sxs-lookup"><span data-stu-id="19859-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="19859-156">Veri koruma anahtarı halka ve dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="19859-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="19859-157">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="19859-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="19859-158">Bu klasör, ağ depolaması tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="19859-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="19859-159">Anahtarlar, bekleme sırasında korunmayan.</span><span class="sxs-lookup"><span data-stu-id="19859-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="19859-160">Bu klasör, anahtar halkası tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="19859-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="19859-161">Hazırlama ve üretim gibi farklı dağıtım yuvaları, anahtar halkası paylaşmayın.</span><span class="sxs-lookup"><span data-stu-id="19859-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="19859-162">Dağıtım yuvası arasında değiştirme, veri korumayı kullanarak herhangi bir sistem önceki yuvasına anahtar halkası kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="19859-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="19859-163">ASP.NET tanımlama bilgisi ara yazılım, veri koruma, tanımlama bilgilerini korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="19859-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="19859-164">Bu, standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu kapatmadan kullanıcılara yol açar.</span><span class="sxs-lookup"><span data-stu-id="19859-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="19859-165">Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="19859-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="19859-166">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="19859-166">Azure Blob Storage</span></span>
* <span data-ttu-id="19859-167">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="19859-167">Azure Key Vault</span></span>
* <span data-ttu-id="19859-168">SQL depolama</span><span class="sxs-lookup"><span data-stu-id="19859-168">SQL store</span></span>
* <span data-ttu-id="19859-169">Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="19859-169">Redis cache</span></span>

<span data-ttu-id="19859-170">Daha fazla bilgi için [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="19859-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="19859-171">ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="19859-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="19859-172">Aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="19859-172">Use one of the following approaches:</span></span>

* <span data-ttu-id="19859-173">[Önizleme site uzantısını yüklemek](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="19859-173">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="19859-174">[Kendi içinde uygulama dağıtmak](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="19859-174">[Deploy the app self-contained](#deploy-the-app-self-contained).</span></span>
* <span data-ttu-id="19859-175">[Docker, kapsayıcılar için Web Apps ile kullanma](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="19859-175">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="19859-176">Önizleme sitesi uzantısını yükle</span><span class="sxs-lookup"><span data-stu-id="19859-176">Install the preview site extension</span></span>

<span data-ttu-id="19859-177">Önizleme site uzantısı kullanarak bir sorun ortaya çıkarsa, bir sorun açın [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="19859-177">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="19859-178">Azure Portalı'ndan, App Service dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="19859-178">From the Azure Portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="19859-179">Web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="19859-179">Select the web app.</span></span>
1. <span data-ttu-id="19859-180">Yönetim bölümlere listesini aşağı kaydırın ve arama kutusuna "ör" türünde **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="19859-180">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="19859-181">Seçin **geliştirme araçları** > **uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="19859-181">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="19859-182">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="19859-182">Select **Add**.</span></span>
1. <span data-ttu-id="19859-183">Seçin **ASP.NET Core &lt;x.y&gt; (x86) çalışma zamanı** uzantısı listeden burada `<x.y>` ASP.NET Core Önizleme sürümü (örneğin, **ASP.NET Core 2.2 (x86) çalışma zamanı**).</span><span class="sxs-lookup"><span data-stu-id="19859-183">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="19859-184">Çalışma zamanı için uygun x86 [framework bağımlı dağıtımları](/dotnet/core/deploying/#framework-dependent-deployments-fdd) kullanan ASP.NET Core modülü tarafından işlem dışı barındırma sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="19859-184">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="19859-185">Seçin **Tamam** yasal koşulları kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="19859-185">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="19859-186">Seçin **Tamam** uzantıyı yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="19859-186">Select **OK** to install the extension.</span></span>

<span data-ttu-id="19859-187">İşlem tamamlandığında, en son .NET Core Önizleme yüklenir.</span><span class="sxs-lookup"><span data-stu-id="19859-187">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="19859-188">Yüklemeyi doğrulama:</span><span class="sxs-lookup"><span data-stu-id="19859-188">Verify the installation:</span></span>

1. <span data-ttu-id="19859-189">Seçin **Gelişmiş Araçlar** altında **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="19859-189">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="19859-190">Seçin **Git** üzerinde **Gelişmiş Araçlar** dikey penceresi.</span><span class="sxs-lookup"><span data-stu-id="19859-190">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="19859-191">Seçin **hata ayıklama konsoluna** > **PowerShell** menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="19859-191">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="19859-192">PowerShell komut isteminde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="19859-192">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="19859-193">ASP.NET Core çalışma zamanı sürümü yerine `<x.y>` komutta:</span><span class="sxs-lookup"><span data-stu-id="19859-193">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="19859-194">Yüklü önizlemesi çalışma zamanı için ASP.NET Core 2.2 ise, komut şöyledir:</span><span class="sxs-lookup"><span data-stu-id="19859-194">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="19859-195">Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.</span><span class="sxs-lookup"><span data-stu-id="19859-195">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="19859-196">Uygulama Hizmetleri uygulama platformu mimarisi (x86/x64) kümesinde **uygulama ayarları** altındaki dikey penceresinde **genel ayarlar** A serisi işlem üzerinde barındırılan veya daha iyi barındırma uygulamaların katmanı.</span><span class="sxs-lookup"><span data-stu-id="19859-196">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="19859-197">Uygulama, işlem içi modunda çalıştırıldığında ve platform mimarisi için 64-bit (x64) yapılandırılmışsa, ASP.NET Core modülü varsa, 64-bit önizlemesi çalışma zamanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="19859-197">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="19859-198">Yükleme **ASP.NET Core &lt;x.y&gt; (x64) çalışma zamanı** uzantısı (örneğin, **ASP.NET Core 2.2 (x64) çalışma zamanı**).</span><span class="sxs-lookup"><span data-stu-id="19859-198">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="19859-199">X64 yükledikten sonra çalışma zamanı Önizleme, yüklemeyi doğrulamak için Kudu PowerShell komut penceresinde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="19859-199">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="19859-200">ASP.NET Core çalışma zamanı sürümü yerine `<x.y>` komutta:</span><span class="sxs-lookup"><span data-stu-id="19859-200">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="19859-201">Yüklü önizlemesi çalışma zamanı için ASP.NET Core 2.2 ise, komut şöyledir:</span><span class="sxs-lookup"><span data-stu-id="19859-201">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="19859-202">Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.</span><span class="sxs-lookup"><span data-stu-id="19859-202">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="19859-203">**ASP.NET Core uzantıları** Azure günlük kaydı etkinleştirme gibi ek işlevler üzerinde Azure App Services, ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="19859-203">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="19859-204">Uzantı, Visual Studio'dan dağıtım yaparken otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="19859-204">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="19859-205">Uzantı yüklü değilse, uygulamayı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="19859-205">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="19859-206">**Bir ARM şablonu ile Önizleme site uzantısı kullanma**</span><span class="sxs-lookup"><span data-stu-id="19859-206">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="19859-207">Bir ARM şablonu, uygulamaları oluşturup dağıtmak için kullanılıyorsa `siteextensions` kaynak türü, bir web uygulamasına site uzantısı eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19859-207">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="19859-208">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="19859-208">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="19859-209">Kendi içinde uygulama dağıtma</span><span class="sxs-lookup"><span data-stu-id="19859-209">Deploy the app self-contained</span></span>

<span data-ttu-id="19859-210">A [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) önizlemesini hedefleyen çalışma zamanı dağıtımda önizlemesi çalışma zamanı taşır.</span><span class="sxs-lookup"><span data-stu-id="19859-210">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="19859-211">Kendi içinde uygulama dağıtırken:</span><span class="sxs-lookup"><span data-stu-id="19859-211">When deploying a self-contained app:</span></span>

* <span data-ttu-id="19859-212">Azure App Service'te site gerektirmeyen [Önizleme site uzantısı](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="19859-212">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="19859-213">Uygulama yayımlama olduğunda daha farklı bir yaklaşım izleyerek yayımlanmalıdır bir [framework bağımlı dağıtım (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="19859-213">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

#### <a name="publish-from-visual-studio"></a><span data-ttu-id="19859-214">Visual Studio'dan yayımlama</span><span class="sxs-lookup"><span data-stu-id="19859-214">Publish from Visual Studio</span></span>

1. <span data-ttu-id="19859-215">Seçin **derleme** > **yayımlama {uygulama-adı}** Visual Studio araç çubuğundan.</span><span class="sxs-lookup"><span data-stu-id="19859-215">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar.</span></span>
1. <span data-ttu-id="19859-216">İçinde **yayımlama hedefi seçin** iletişim kutusunda onaylayın **App Service** seçilir.</span><span class="sxs-lookup"><span data-stu-id="19859-216">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="19859-217">Seçin **Gelişmiş**.</span><span class="sxs-lookup"><span data-stu-id="19859-217">Select **Advanced**.</span></span> <span data-ttu-id="19859-218">**Yayımla** iletişim kutusu açılır.</span><span class="sxs-lookup"><span data-stu-id="19859-218">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="19859-219">İçinde **Yayımla** iletişim:</span><span class="sxs-lookup"><span data-stu-id="19859-219">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="19859-220">Onaylayın **yayın** yapılandırması seçili.</span><span class="sxs-lookup"><span data-stu-id="19859-220">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="19859-221">Açık **dağıtım modu** aşağı açılan listesinden **müstakil**.</span><span class="sxs-lookup"><span data-stu-id="19859-221">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="19859-222">Hedef çalışma zamanını şuradan seçin **hedef çalışma zamanı** aşağı açılan listesi.</span><span class="sxs-lookup"><span data-stu-id="19859-222">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="19859-223">Varsayılan, `win-x86` değeridir.</span><span class="sxs-lookup"><span data-stu-id="19859-223">The default is `win-x86`.</span></span>
   * <span data-ttu-id="19859-224">Ek dosyaları dağıtım kaldırmanız gerekirse, açık **dosya yayımlama seçeneği** ve hedefteki ek dosyaları kaldırmak için bu onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="19859-224">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="19859-225">Seçin **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="19859-225">Select **Save**.</span></span>
1. <span data-ttu-id="19859-226">Yeni bir site veya mevcut bir site Yayımlama Sihirbazı'nın kalan istemleri izleyerek güncelleştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="19859-226">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

#### <a name="publish-using-command-line-interface-cli-tools"></a><span data-ttu-id="19859-227">Komut satırı arabirimi (CLI) araçlarını kullanarak yayımla</span><span class="sxs-lookup"><span data-stu-id="19859-227">Publish using command-line interface (CLI) tools</span></span>

1. <span data-ttu-id="19859-228">Proje dosyasında bir veya daha fazla belirtin [çalışma zamanı tanımlayıcılarının (RID'ler)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="19859-228">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="19859-229">Kullanma `<RuntimeIdentifier>` tek RID veya kullanın (tekil) `<RuntimeIdentifiers>` RID'ler noktalı virgülle ayrılmış bir listesini sağlamak üzere (çoğul).</span><span class="sxs-lookup"><span data-stu-id="19859-229">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="19859-230">Aşağıdaki örnekte, `win-x86` RID belirtilir:</span><span class="sxs-lookup"><span data-stu-id="19859-230">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```
1. <span data-ttu-id="19859-231">Bir komut kabuğu'ndan, sürüm yapılandırmasında ana bilgisayarın çalışma zamanı ile uygulamayı yayımlama [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu.</span><span class="sxs-lookup"><span data-stu-id="19859-231">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="19859-232">Aşağıdaki örnekte, uygulama için yayımlanan `win-x86` RID.</span><span class="sxs-lookup"><span data-stu-id="19859-232">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="19859-233">Sağlanan RID `--runtime` seçeneği sağlanmalıdır `<RuntimeIdentifier>` (veya `<RuntimeIdentifiers>`) proje dosyasındaki özellik.</span><span class="sxs-lookup"><span data-stu-id="19859-233">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```
1. <span data-ttu-id="19859-234">İçeriği Taşı *bin/bırakma / {TARGET FRAMEWORK} / {çalışma zamanı TANIMLAYICISI} / publish* App Service'te bir siteye dizin.</span><span class="sxs-lookup"><span data-stu-id="19859-234">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="19859-235">Docker, kapsayıcılar için Web Apps ile kullanma</span><span class="sxs-lookup"><span data-stu-id="19859-235">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="19859-236">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) en son içeren Docker görüntüleri önizleme.</span><span class="sxs-lookup"><span data-stu-id="19859-236">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="19859-237">Görüntü, temel görüntü olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19859-237">The images can be used as a base image.</span></span> <span data-ttu-id="19859-238">Görüntüyü kullanın ve kapsayıcılar için Web Apps için normal olarak dağıtın.</span><span class="sxs-lookup"><span data-stu-id="19859-238">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="19859-239">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="19859-239">Additional resources</span></span>

* [<span data-ttu-id="19859-240">Web Apps'e genel bakış (5 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="19859-240">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="19859-241">Azure App Service: Konak için en iyi, .NET uygulamalarınızı (55 dakikalık genel bakış videosu) yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="19859-241">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="19859-242">Azure Friday: Azure App Service tanılama ve sorun giderme deneyimini (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="19859-242">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="19859-243">Azure App Service tanılama genel bakış</span><span class="sxs-lookup"><span data-stu-id="19859-243">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="19859-244">Windows Server üzerinde Azure App Service kullanan [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="19859-244">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="19859-245">Aşağıdaki konular temel alınan IIS teknolojiye ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="19859-245">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="19859-246">Microsoft TechNet Kitaplığı'de: Windows Server</span><span class="sxs-lookup"><span data-stu-id="19859-246">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
