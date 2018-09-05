---
title: Azure App Service'te ASP.NET Core barındırma
author: guardrex
description: Faydalı kaynaklara bağlantılarla Azure App Service'te ASP.NET Core uygulamaları barındırmak nasıl keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: bc2a686c5ddc44fded135c9eed5caf676218773a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312076"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="5acd4-103">Azure App Service'te ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="5acd4-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="5acd4-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) olduğu bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) ASP.NET Core web uygulamalarını barındırmak için dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="5acd4-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="5acd4-105">Faydalı kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5acd4-105">Useful resources</span></span>

<span data-ttu-id="5acd4-106">Azure [Web Apps belgeleri](/azure/app-service/) Azure uygulamaları belgeler, öğreticiler, örnekler, nasıl yapılır kılavuzları ve diğer kaynaklar için platformdur.</span><span class="sxs-lookup"><span data-stu-id="5acd4-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="5acd4-107">ASP.NET Core uygulamaları barındırmak için ilgilidir iki önemli öğreticiler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5acd4-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="5acd4-108">Hızlı Başlangıç: Azure'da bir ASP.NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="5acd4-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="5acd4-109">Visual Studio, oluşturmak ve Windows üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="5acd4-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="5acd4-110">Hızlı Başlangıç: Linux üzerinde App Service'te .NET Core web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="5acd4-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="5acd4-111">Oluşturmak ve Linux üzerinde Azure App Service'e bir ASP.NET Core web uygulaması dağıtmak için komut satırını kullanın.</span><span class="sxs-lookup"><span data-stu-id="5acd4-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="5acd4-112">ASP.NET Core belgelerinde aşağıdaki makalelere kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5acd4-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="5acd4-113">Visual Studio ile Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="5acd4-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="5acd4-114">Visual Studio kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="5acd4-115">CLI araçları ile Azure’a yayımlama</span><span class="sxs-lookup"><span data-stu-id="5acd4-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="5acd4-116">Azure App Service'e Git komut satırı istemcisini kullanarak bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="5acd4-117">Visual Studio ve Git ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="5acd4-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="5acd4-118">Visual Studio kullanarak ASP.NET Core web uygulaması oluşturmayı öğrenin ve Git kullanarak sürekli dağıtım için Azure App Service'e dağıtın.</span><span class="sxs-lookup"><span data-stu-id="5acd4-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="5acd4-119">VSTS ile Azure’a sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="5acd4-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="5acd4-120">ASP.NET Core uygulaması için bir CI derlemesi ayarlayın ve ardından bir Azure App Service'e sürekli dağıtım yayın oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5acd4-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="5acd4-121">Azure Web uygulama korumalı alanı</span><span class="sxs-lookup"><span data-stu-id="5acd4-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="5acd4-122">Azure App Service Azure uygulama platformu tarafından zorlanan çalışma zamanı yürütme sınırlamaları keşfedin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="5acd4-123">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="5acd4-123">Application configuration</span></span>

<span data-ttu-id="5acd4-124">ASP.NET Core 2.0 veya sonraki sürümlerde, aşağıdaki NuGet paketlerini Azure App Service'e dağıtılan uygulamalar için otomatik günlük tutma özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="5acd4-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="5acd4-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) kullanan [Ihostingstartup](xref:fundamentals/configuration/platform-specific-configuration) Azure App Service ile ASP.NET Core açık yukarı tümleştirmesi sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="5acd4-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="5acd4-126">Eklenen günlüğe kaydetme özelliklerini tarafından sağlanan `Microsoft.AspNetCore.AzureAppServicesIntegration` paket.</span><span class="sxs-lookup"><span data-stu-id="5acd4-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="5acd4-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span><span class="sxs-lookup"><span data-stu-id="5acd4-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="5acd4-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) Günlükçü uygulamalarını Azure App Service tanılama günlüklerini ve günlük özellikleri akışı desteklemek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="5acd4-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="5acd4-129">.NET Core'u hedefleyen ve bunlara başvurma [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), paketleri zaten dahildir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="5acd4-130">Eksik paketleri yeni gelen [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5acd4-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="5acd4-131">.NET Framework'ü hedefleyen veya başvuru `Microsoft.AspNetCore.App` metapackage, tek tek günlük paketleri başvuru.</span><span class="sxs-lookup"><span data-stu-id="5acd4-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="5acd4-132">Azure portalını kullanarak uygulama yapılandırması geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="5acd4-132">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="5acd4-133">**Uygulama ayarları** alanının **uygulama ayarları** dikey penceresinde, uygulama için ortam değişkenlerini ayarlamak için izin verir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-133">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="5acd4-134">Ortam değişkenleri tarafından kullanılabilir [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5acd4-134">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="5acd4-135">Bir uygulama kullandığında [Web ana bilgisayarı](xref:fundamentals/host/web-host) kullanarak ana bilgisayar oluşturur [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), ana bilgisayar yapılandırma ortam değişkenlerini kullanma `ASPNETCORE_` önek.</span><span class="sxs-lookup"><span data-stu-id="5acd4-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="5acd4-136">Daha fazla bilgi için <xref:fundamentals/host/web-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5acd4-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="5acd4-137">Bir uygulama kullandığında [genel ana bilgisayar](xref:fundamentals/host/generic-host), ortam değişkenleri uygulamanın yapılandırmasını varsayılan olarak yüklü değildir ve geliştirici tarafından yapılandırma sağlayıcısı eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="5acd4-138">Yapılandırma sağlayıcısı eklendiğinde Geliştirici ortamı değişken önek belirler.</span><span class="sxs-lookup"><span data-stu-id="5acd4-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="5acd4-139">Daha fazla bilgi için <xref:fundamentals/host/generic-host> ve [ortam değişkenlerini yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="5acd4-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="5acd4-140">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="5acd4-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="5acd4-141">İletilen üstbilgileri ara yazılım ve ASP.NET Core modülü yapılandırır IIS tümleştirme Ara şema (HTTP/HTTPS) ve isteğin geldiği uzak IP adresine iletecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="5acd4-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="5acd4-142">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="5acd4-143">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="5acd4-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="5acd4-144">İzleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="5acd4-144">Monitoring and logging</span></span>

<span data-ttu-id="5acd4-145">İzleme, günlüğe kaydetme ve sorun giderme bilgileri için aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="5acd4-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="5acd4-146">Nasıl yapılır: Azure App Service'te uygulamaları izleme</span><span class="sxs-lookup"><span data-stu-id="5acd4-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="5acd4-147">Kotalar ve uygulamaları ve App Service planları için ölçümleri gözden geçirmeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="5acd4-148">Azure App Service'te web apps için tanılama günlüğünü etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="5acd4-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="5acd4-149">HTTP durum kodları, başarısız istekler ve web sunucusu etkinliğini için tanılama günlüğüne kaydetme erişimi nasıl etkinleştirileceği keşfedin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="5acd4-150">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="5acd4-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="5acd4-151">ASP.NET Core uygulamalarında hata işleme için genel yaklaşımları anlayın.</span><span class="sxs-lookup"><span data-stu-id="5acd4-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="5acd4-152">Azure App Service’te uygulama sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="5acd4-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="5acd4-153">ASP.NET Core uygulamaları ile Azure App Service dağıtımı ile ilgili sorunları tanılamayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="5acd4-154">Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="5acd4-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="5acd4-155">Sık karşılaşılan dağıtım yapılandırma hatalarını sorun giderme önerilerine ile Azure App Service/IIS tarafından barındırılan uygulamalar için bkz.</span><span class="sxs-lookup"><span data-stu-id="5acd4-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="5acd4-156">Veri koruma anahtarı halka ve dağıtım yuvaları</span><span class="sxs-lookup"><span data-stu-id="5acd4-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="5acd4-157">[Veri koruma anahtarları](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) için kalıcı *%HOME%\ASP.NET\DataProtection-Keys* klasör.</span><span class="sxs-lookup"><span data-stu-id="5acd4-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="5acd4-158">Bu klasör, ağ depolaması tarafından desteklenir ve uygulamayı barındıran tüm makinelerde eşitlenir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="5acd4-159">Anahtarlar, bekleme sırasında korunmayan.</span><span class="sxs-lookup"><span data-stu-id="5acd4-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="5acd4-160">Bu klasör, anahtar halkası tek dağıtım yuvasındaki bir uygulamanın tüm örnekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="5acd4-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="5acd4-161">Hazırlama ve üretim gibi farklı dağıtım yuvaları, anahtar halkası paylaşmayın.</span><span class="sxs-lookup"><span data-stu-id="5acd4-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="5acd4-162">Dağıtım yuvası arasında değiştirme, veri korumayı kullanarak herhangi bir sistem önceki yuvasına anahtar halkası kullanarak depolanan verilerin şifresini çözmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="5acd4-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="5acd4-163">ASP.NET tanımlama bilgisi ara yazılım, veri koruma, tanımlama bilgilerini korumak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="5acd4-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="5acd4-164">Bu, standart ASP.NET tanımlama bilgisi Ara kullanan bir uygulamanın oturumunu kapatmadan kullanıcılara yol açar.</span><span class="sxs-lookup"><span data-stu-id="5acd4-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="5acd4-165">Yuva bağımsız anahtar halkası çözümü için bir dış anahtar halkası sağlayıcısı gibi kullanın:</span><span class="sxs-lookup"><span data-stu-id="5acd4-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="5acd4-166">Azure Blob Depolama</span><span class="sxs-lookup"><span data-stu-id="5acd4-166">Azure Blob Storage</span></span>
* <span data-ttu-id="5acd4-167">Azure anahtar kasası</span><span class="sxs-lookup"><span data-stu-id="5acd4-167">Azure Key Vault</span></span>
* <span data-ttu-id="5acd4-168">SQL depolama</span><span class="sxs-lookup"><span data-stu-id="5acd4-168">SQL store</span></span>
* <span data-ttu-id="5acd4-169">Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="5acd4-169">Redis cache</span></span>

<span data-ttu-id="5acd4-170">Daha fazla bilgi için [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="5acd4-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="5acd4-171">ASP.NET Core Önizleme sürümü, Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="5acd4-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="5acd4-172">ASP.NET Core Önizleme uygulamalarını Azure App Service için aşağıdaki yaklaşımlardan ile dağıtılabilir:</span><span class="sxs-lookup"><span data-stu-id="5acd4-172">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="5acd4-173">[Önizleme sitesi uzantısını yükle](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="5acd4-173">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="5acd4-174">Docker, kapsayıcılar için Web Apps ile kullanma</span><span class="sxs-lookup"><span data-stu-id="5acd4-174">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="5acd4-175">Önizleme sitesi uzantısını yükle</span><span class="sxs-lookup"><span data-stu-id="5acd4-175">Install the preview site extension</span></span>

<span data-ttu-id="5acd4-176">Önizleme site uzantısı kullanarak bir sorun ortaya çıkarsa, bir sorun açın [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="5acd4-176">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="5acd4-177">Azure portalda, App Service dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-177">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="5acd4-178">Web uygulamasını seçin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-178">Select the web app.</span></span>
1. <span data-ttu-id="5acd4-179">Yönetim bölümlere listesini aşağı kaydırın ve arama kutusuna "ör" türünde **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="5acd4-179">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="5acd4-180">Seçin **geliştirme araçları** > **uzantıları**.</span><span class="sxs-lookup"><span data-stu-id="5acd4-180">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="5acd4-181">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="5acd4-181">Select **Add**.</span></span>
1. <span data-ttu-id="5acd4-182">Seçin **ASP.NET Core &lt;x.y&gt; (x86) çalışma zamanı** uzantısı listeden burada `<x.y>` ASP.NET Core Önizleme sürümü (örneğin, **ASP.NET Core 2.2 (x86) çalışma zamanı**).</span><span class="sxs-lookup"><span data-stu-id="5acd4-182">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="5acd4-183">Çalışma zamanı için uygun x86 [framework bağımlı dağıtımları](/dotnet/core/deploying/#framework-dependent-deployments-fdd) kullanan ASP.NET Core modülü tarafından işlem dışı barındırma sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="5acd4-183">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="5acd4-184">Seçin **Tamam** yasal koşulları kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="5acd4-184">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="5acd4-185">Seçin **Tamam** uzantıyı yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="5acd4-185">Select **OK** to install the extension.</span></span>

<span data-ttu-id="5acd4-186">İşlem tamamlandığında, en son .NET Core Önizleme yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-186">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="5acd4-187">Yüklemeyi doğrulama:</span><span class="sxs-lookup"><span data-stu-id="5acd4-187">Verify the installation:</span></span>

1. <span data-ttu-id="5acd4-188">Seçin **Gelişmiş Araçlar** altında **geliştirme araçları**.</span><span class="sxs-lookup"><span data-stu-id="5acd4-188">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="5acd4-189">Seçin **Git** üzerinde **Gelişmiş Araçlar** dikey penceresi.</span><span class="sxs-lookup"><span data-stu-id="5acd4-189">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="5acd4-190">Seçin **hata ayıklama konsoluna** > **PowerShell** menü öğesi.</span><span class="sxs-lookup"><span data-stu-id="5acd4-190">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="5acd4-191">PowerShell komut isteminde aşağıdaki komutu yürütün.</span><span class="sxs-lookup"><span data-stu-id="5acd4-191">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="5acd4-192">ASP.NET Core çalışma zamanı sürümü yerine `<x.y>` komutta:</span><span class="sxs-lookup"><span data-stu-id="5acd4-192">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="5acd4-193">Yüklü önizlemesi çalışma zamanı için ASP.NET Core 2.2 ise, komut şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5acd4-193">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="5acd4-194">Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.</span><span class="sxs-lookup"><span data-stu-id="5acd4-194">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="5acd4-195">Uygulama Hizmetleri uygulama platformu mimarisi (x86/x64) kümesinde **uygulama ayarları** altındaki dikey penceresinde **genel ayarlar** A serisi işlem üzerinde barındırılan veya daha iyi barındırma uygulamaların katmanı.</span><span class="sxs-lookup"><span data-stu-id="5acd4-195">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="5acd4-196">Uygulama, işlem içi modunda çalıştırıldığında ve platform mimarisi için 64-bit (x64) yapılandırılmışsa, ASP.NET Core modülü varsa, 64-bit önizlemesi çalışma zamanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="5acd4-196">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="5acd4-197">Yükleme **ASP.NET Core &lt;x.y&gt; (x64) çalışma zamanı** uzantısı (örneğin, **ASP.NET Core 2.2 (x64) çalışma zamanı**).</span><span class="sxs-lookup"><span data-stu-id="5acd4-197">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="5acd4-198">X64 yükledikten sonra çalışma zamanı Önizleme, yüklemeyi doğrulamak için Kudu PowerShell komut penceresinde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5acd4-198">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="5acd4-199">ASP.NET Core çalışma zamanı sürümü yerine `<x.y>` komutta:</span><span class="sxs-lookup"><span data-stu-id="5acd4-199">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="5acd4-200">Yüklü önizlemesi çalışma zamanı için ASP.NET Core 2.2 ise, komut şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5acd4-200">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="5acd4-201">Komut döndürür `True` olduğunda önizlemesi çalışma zamanı yüklü x64.</span><span class="sxs-lookup"><span data-stu-id="5acd4-201">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="5acd4-202">**ASP.NET Core uzantıları** Azure günlük kaydı etkinleştirme gibi ek işlevler üzerinde Azure App Services, ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="5acd4-202">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="5acd4-203">Uzantı, Visual Studio'dan dağıtım yaparken otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-203">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="5acd4-204">Uzantı yüklü değilse, uygulamayı yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-204">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="5acd4-205">**Bir ARM şablonu ile Önizleme site uzantısı kullanma**</span><span class="sxs-lookup"><span data-stu-id="5acd4-205">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="5acd4-206">Bir ARM şablonu, uygulamaları oluşturup dağıtmak için kullanılıyorsa `siteextensions` kaynak türü, bir web uygulamasına site uzantısı eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-206">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="5acd4-207">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5acd4-207">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="5acd4-208">Docker, kapsayıcılar için Web Apps ile kullanma</span><span class="sxs-lookup"><span data-stu-id="5acd4-208">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="5acd4-209">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) en son içeren Docker görüntüleri önizleme.</span><span class="sxs-lookup"><span data-stu-id="5acd4-209">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="5acd4-210">Görüntü, temel görüntü olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5acd4-210">The images can be used as a base image.</span></span> <span data-ttu-id="5acd4-211">Görüntüyü kullanın ve kapsayıcılar için Web Apps için normal olarak dağıtın.</span><span class="sxs-lookup"><span data-stu-id="5acd4-211">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5acd4-212">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5acd4-212">Additional resources</span></span>

* [<span data-ttu-id="5acd4-213">Web Apps'e genel bakış (5 dakikalık genel bakış videosu)</span><span class="sxs-lookup"><span data-stu-id="5acd4-213">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="5acd4-214">Azure App Service: Konak için en iyi, .NET uygulamalarınızı (55 dakikalık genel bakış videosu) yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="5acd4-214">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="5acd4-215">Azure Friday: Azure App Service tanılama ve sorun giderme deneyimini (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="5acd4-215">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="5acd4-216">Azure App Service tanılama genel bakış</span><span class="sxs-lookup"><span data-stu-id="5acd4-216">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="5acd4-217">Windows Server üzerinde Azure App Service kullanan [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="5acd4-217">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="5acd4-218">Aşağıdaki konular temel alınan IIS teknolojiye ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="5acd4-218">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="5acd4-219">Microsoft TechNet Kitaplığı'de: Windows Server</span><span class="sxs-lookup"><span data-stu-id="5acd4-219">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
