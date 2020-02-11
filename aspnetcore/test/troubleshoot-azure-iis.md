---
title: Azure App Service ve IIS 'de ASP.NET Core sorunlarını giderme
author: guardrex
description: ASP.NET Core uygulamalarının Azure App Service ve Internet Information Services (IIS) dağıtımlarıyla ilgili sorunları tanılamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: a5cd17e46126828c6bc8436ccaaca28edb2573d0
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114854"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="662e9-103">Azure App Service ve IIS 'de ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="662e9-104">, [Luke Latham](https://github.com/guardrex) ve, [kotalik](https://github.com/jkotalik) tarafından</span><span class="sxs-lookup"><span data-stu-id="662e9-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="662e9-105">Bu makalede, bir uygulama Azure App Service veya IIS 'ye dağıtıldığında hataların nasıl tanılanacağı hakkında genel uygulama başlatma hataları ve yönergeleri hakkında bilgi verilmektedir:</span><span class="sxs-lookup"><span data-stu-id="662e9-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="662e9-106">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="662e9-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="662e9-107">Ortak Başlangıç HTTP durum kodu senaryolarını açıklar.</span><span class="sxs-lookup"><span data-stu-id="662e9-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="662e9-108">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="662e9-109">Azure App Service dağıtılan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="662e9-110">IIS üzerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="662e9-111">IIS 'ye dağıtılan veya IIS Express yerel olarak çalışan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="662e9-112">Bu kılavuz hem Windows Server hem de Windows masaüstü dağıtımları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="662e9-113">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="662e9-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="662e9-114">Önemli güncelleştirmeler gerçekleştirirken veya paket sürümlerini değiştirirken ne yapmanız gerektiğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="662e9-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="662e9-115">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="662e9-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="662e9-116">Ek sorun giderme konularını listeler.</span><span class="sxs-lookup"><span data-stu-id="662e9-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="662e9-117">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="662e9-117">App startup errors</span></span>

<span data-ttu-id="662e9-118">Visual Studio 'da bir ASP.NET Core projesi, hata ayıklama sırasında [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) barındırmak için varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="662e9-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="662e9-119">*502,5-Işlem hatası* veya yerel olarak hata ayıklarken oluşan *500,30-başlatma hatası* , bu konudaki öneri kullanılarak tanılanabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="662e9-120">403,14 yasak</span><span class="sxs-lookup"><span data-stu-id="662e9-120">403.14 Forbidden</span></span>

<span data-ttu-id="662e9-121">Uygulama başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-121">The app fails to start.</span></span> <span data-ttu-id="662e9-122">Aşağıdaki hata günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="662e9-122">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="662e9-123">Hata genellikle barındırma sisteminde, aşağıdaki senaryolardan birini içeren bozuk bir dağıtım nedeniyle oluşur:</span><span class="sxs-lookup"><span data-stu-id="662e9-123">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="662e9-124">Uygulama, barındırma sisteminde yanlış klasöre dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-124">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="662e9-125">Dağıtım işlemi, uygulamanın tüm dosyalarını ve klasörlerini barındırma sistemindeki dağıtım klasörüne taşıyamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-125">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="662e9-126">*Web. config* dosyası dağıtımda yok veya *Web. config* dosyası içerikleri hatalı biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="662e9-126">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="662e9-127">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="662e9-127">Perform the following steps:</span></span>

1. <span data-ttu-id="662e9-128">Tüm dosya ve klasörleri barındırma sistemindeki dağıtım klasöründen silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-128">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="662e9-129">Visual Studio, PowerShell veya el ile dağıtım gibi normal dağıtım yönteminizi kullanarak, uygulamanın *Yayımlama* klasörünün içeriğini barındırma sistemine yeniden dağıtın:</span><span class="sxs-lookup"><span data-stu-id="662e9-129">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="662e9-130">*Web. config* dosyasının dağıtımda mevcut olduğunu ve içeriğinin doğru olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-130">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="662e9-131">Azure App Service barındırırken, uygulamanın `D:\home\site\wwwroot` klasörüne dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-131">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="662e9-132">Uygulama IIS tarafından barındırılıyorsa, uygulamanın **IIS yöneticisinin** **temel ayarlarında**gösterilen IIS **fiziksel yoluna** dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-132">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="662e9-133">Barındırma sistemindeki dağıtımı projenin *Yayımla* klasörünün içeriğiyle karşılaştırarak uygulamanın tüm dosya ve klasörlerinin dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-133">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="662e9-134">Yayımlanan ASP.NET Core uygulamasının düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="662e9-134">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="662e9-135">*Web. config* dosyası hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="662e9-135">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="662e9-136">500 İç Sunucu Hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-136">500 Internal Server Error</span></span>

<span data-ttu-id="662e9-137">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="662e9-137">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="662e9-138">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-138">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="662e9-139">Yanıtta içerik yok olabilir veya Yanıt, tarayıcıda *500 Iç sunucu hatası* olarak görünebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-139">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="662e9-140">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="662e9-140">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="662e9-141">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-141">From the server's perspective, that's correct.</span></span> <span data-ttu-id="662e9-142">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-142">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="662e9-143">Uygulamayı sunucuda bir komut isteminde çalıştırın veya sorunu gidermek için ASP.NET Core modülü stdout günlüğünü etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="662e9-143">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="662e9-144">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-144">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="662e9-145">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-145">The worker process fails.</span></span> <span data-ttu-id="662e9-146">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-146">The app doesn't start.</span></span>

<span data-ttu-id="662e9-147">[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) bileşenleri yüklenirken bilinmeyen bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="662e9-147">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="662e9-148">Aşağıdaki eylemlerden birini gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="662e9-148">Take one of the following actions:</span></span>

* <span data-ttu-id="662e9-149">[Microsoft desteği](https://support.microsoft.com/oas/default.aspx?prid=15832) iletişim kurun ( **Geliştirici Araçları** ve **ASP.NET Core**' i seçin).</span><span class="sxs-lookup"><span data-stu-id="662e9-149">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="662e9-150">Stack Overflow soru sorun.</span><span class="sxs-lookup"><span data-stu-id="662e9-150">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="662e9-151">[GitHub deponuzda](https://github.com/dotnet/AspNetCore)bir sorun yapın.</span><span class="sxs-lookup"><span data-stu-id="662e9-151">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="662e9-152">500.30 işlemdeki başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-152">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="662e9-153">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-153">The worker process fails.</span></span> <span data-ttu-id="662e9-154">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-154">The app doesn't start.</span></span>

<span data-ttu-id="662e9-155">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) .NET Core CLR 'yi işlem içi başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-155">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="662e9-156">İşlem başlatma hatasının nedeni genellikle uygulama olay günlüğündeki girişlerden ve ASP.NET Core modülü stdout günlüğünde belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-156">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="662e9-157">Yaygın hata koşulları:</span><span class="sxs-lookup"><span data-stu-id="662e9-157">Common failure conditions:</span></span>

* <span data-ttu-id="662e9-158">Mevcut olmayan ASP.NET Core paylaşılan çerçevesinin bir sürümünün hedeflenmesi nedeniyle uygulama yanlış yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="662e9-158">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="662e9-159">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-159">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="662e9-160">Azure Key Vault kullanarak Key Vault izinlerin bulunmaması.</span><span class="sxs-lookup"><span data-stu-id="662e9-160">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="662e9-161">Doğru izinlerin verildiğinden emin olmak için hedeflenen Key Vault erişim ilkelerini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="662e9-161">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="662e9-162">500,31 ANCM yerel bağımlılıklar bulunamadı</span><span class="sxs-lookup"><span data-stu-id="662e9-162">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="662e9-163">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-163">The worker process fails.</span></span> <span data-ttu-id="662e9-164">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-164">The app doesn't start.</span></span>

<span data-ttu-id="662e9-165">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) , .NET Core çalışma zamanını işlem içinde başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-165">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="662e9-166">Bu başlatma hatasının en yaygın nedeni, `Microsoft.NETCore.App` veya `Microsoft.AspNetCore.App` çalışma zamanının yüklenmemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-166">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="662e9-167">Uygulama, hedef ASP.NET Core 3,0 ' ye dağıtılmışsa ve bu sürüm makinede yoksa, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-167">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="662e9-168">Örnek bir hata iletisi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="662e9-168">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="662e9-169">Hata iletisi, yüklü tüm .NET Core sürümlerini ve uygulama tarafından istenen sürümü listeler.</span><span class="sxs-lookup"><span data-stu-id="662e9-169">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="662e9-170">Bu hatayı onarmak için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="662e9-170">To fix this error, either:</span></span>

* <span data-ttu-id="662e9-171">Uygun .NET Core sürümünü makineye yükler.</span><span class="sxs-lookup"><span data-stu-id="662e9-171">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="662e9-172">Uygulamayı, makinede bulunan .NET Core 'un bir sürümünü hedefleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="662e9-172">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="662e9-173">Uygulamayı [kendi kendine kapsanan bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)olarak yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-173">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="662e9-174">Geliştirme aşamasında çalışırken (`ASPNETCORE_ENVIRONMENT` ortam değişkeni `Development`olarak ayarlandığında), HTTP yanıtına belirli bir hata yazılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-174">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="662e9-175">İşlem başlatma hatasının nedeni uygulama olay günlüğünde de bulunur.</span><span class="sxs-lookup"><span data-stu-id="662e9-175">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="662e9-176">500,32 ANCM dll yüklenemedi</span><span class="sxs-lookup"><span data-stu-id="662e9-176">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="662e9-177">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-177">The worker process fails.</span></span> <span data-ttu-id="662e9-178">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-178">The app doesn't start.</span></span>

<span data-ttu-id="662e9-179">Bu hatanın en yaygın nedeni, uygulamanın uyumsuz bir işlemci mimarisi için yayımlanmakta olması olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-179">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="662e9-180">Çalışan işlemi 32 bitlik bir uygulama olarak çalışıyorsa ve uygulama 64 bit hedef için yayımlandıysa, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-180">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="662e9-181">Bu hatayı onarmak için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="662e9-181">To fix this error, either:</span></span>

* <span data-ttu-id="662e9-182">Çalışan işlemle aynı işlemci mimarisi için uygulamayı yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-182">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="662e9-183">Uygulamayı [çerçeveye bağlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-executables-fde)olarak yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-183">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="662e9-184">500,33 ANCM Istek Işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-184">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="662e9-185">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-185">The worker process fails.</span></span> <span data-ttu-id="662e9-186">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-186">The app doesn't start.</span></span>

<span data-ttu-id="662e9-187">Uygulama `Microsoft.AspNetCore.App` çerçevesine başvurmadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-187">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="662e9-188">Yalnızca `Microsoft.AspNetCore.App` çerçevesini hedefleyen uygulamalar [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module)tarafından barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-188">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="662e9-189">Bu hatayı düzeltemedi, uygulamanın `Microsoft.AspNetCore.App` çerçevesini hedeflediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-189">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="662e9-190">Uygulamanın hedeflediği çerçeveyi doğrulamak için `.runtimeconfig.json` denetleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-190">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="662e9-191">500,34 ANCM karışık barındırma modelleri desteklenmez</span><span class="sxs-lookup"><span data-stu-id="662e9-191">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="662e9-192">Çalışan işlem, aynı işlemde hem işlem içi uygulama hem de işlem dışı bir uygulama çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-192">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="662e9-193">Bu hatayı onarmak için uygulamaları ayrı IIS uygulama havuzlarında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-193">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="662e9-194">500,35 ANCM birden çok işlem Içi uygulama aynı Işlemde</span><span class="sxs-lookup"><span data-stu-id="662e9-194">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="662e9-195">Çalışan işlemi aynı işlemde birden çok işlem içi uygulama çalıştıramıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-195">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="662e9-196">Bu hatayı onarmak için uygulamaları ayrı IIS uygulama havuzlarında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-196">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="662e9-197">500,36 ANCM Işlem dışı Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-197">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="662e9-198">İşlem dışı istek işleyicisi, *aspnetcorev2_outofprocess. dll*, *aspnetcorev2. dll* dosyasının yanında değildir.</span><span class="sxs-lookup"><span data-stu-id="662e9-198">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="662e9-199">Bu, [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)bozuk bir yüklemesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="662e9-199">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="662e9-200">Bu hatayı gidermek için [.NET Core barındırma paketi](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (IIS için) veya Visual Studio (IIS Express için) yüklemesini onarın.</span><span class="sxs-lookup"><span data-stu-id="662e9-200">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="662e9-201">500,37 ANCM başlangıç zamanı sınırı Içinde başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="662e9-201">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="662e9-202">ANCM, kısımları başlangıç süresi sınırı içinde başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-202">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="662e9-203">Varsayılan olarak, zaman aşımı 120 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="662e9-203">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="662e9-204">Aynı makinede çok sayıda uygulama başlatılırken bu hata oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-204">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="662e9-205">Başlangıç sırasında sunucuda CPU/bellek kullanımı artışlarını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-205">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="662e9-206">Birden çok uygulamanın başlatma işlemini şaşırtmayı yapmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-206">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="662e9-207">502.5 işlem hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-207">502.5 Process Failure</span></span>

<span data-ttu-id="662e9-208">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-208">The worker process fails.</span></span> <span data-ttu-id="662e9-209">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-209">The app doesn't start.</span></span>

<span data-ttu-id="662e9-210">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) çalışan işlemini başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-210">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="662e9-211">İşlem başlatma hatasının nedeni genellikle uygulama olay günlüğündeki girişlerden ve ASP.NET Core modülü stdout günlüğünde belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-211">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="662e9-212">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="662e9-212">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="662e9-213">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-213">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="662e9-214">*Paylaşılan çerçeve* , makinede yüklü olan ve `Microsoft.AspNetCore.App`gibi bir metapackage tarafından başvurulan derleme ( *. dll* dosyaları) kümesidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-214">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="662e9-215">Metapackage başvurusu, gerekli en düşük sürümü belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-215">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="662e9-216">Daha fazla bilgi için bkz. [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="662e9-216">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="662e9-217">Bir barındırma veya uygulamanın yanlış yapılandırılması, çalışan işleminin başarısız olmasına neden olduğunda, *502,5 Işlem hata* hatası sayfası döndürülür:</span><span class="sxs-lookup"><span data-stu-id="662e9-217">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="662e9-218">Uygulama (hata kodu: '0x800700c1') başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-218">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="662e9-219">Uygulamanın derlemesi ( *. dll*) yüklenemediğinden uygulama başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-219">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="662e9-220">W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-220">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="662e9-221">Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="662e9-221">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="662e9-222">IIS yöneticisinin **uygulama havuzlarında**uygulama havuzunu seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-222">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="662e9-223">**Eylemler** panelinde **uygulama havuzunu Düzenle** altında **Gelişmiş ayarlar** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-223">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="662e9-224">**Enable 32 bit uygulamalarını**ayarla:</span><span class="sxs-lookup"><span data-stu-id="662e9-224">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="662e9-225">32-bit (x86) bir uygulama dağıtıyorsanız, değeri `True`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-225">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="662e9-226">64 bit (x64) uygulaması dağıtıyorsanız, değeri `False`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-226">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="662e9-227">Proje dosyasındaki `<Platform>` MSBuild özelliği ile uygulamanın yayınlanan bit durumuyla ilgili bir çakışma olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-227">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="662e9-228">Bağlantı sıfırlama</span><span class="sxs-lookup"><span data-stu-id="662e9-228">Connection reset</span></span>

<span data-ttu-id="662e9-229">Üstbilgiler gönderildikten sonra bir hata oluşursa, bir hata oluştuğunda sunucunun **500 Iç sunucu hatası** gönderebilmesi için çok geç olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-229">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="662e9-230">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="662e9-230">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="662e9-231">Bu tür bir hata, istemcide bir *bağlantı sıfırlama* hatası olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-231">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="662e9-232">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataların giderilmesine yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-232">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="662e9-233">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="662e9-233">Default startup limits</span></span>

<span data-ttu-id="662e9-234">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) varsayılan bir *StartupTimeLimit* 120 saniye ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-234">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="662e9-235">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-235">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="662e9-236">Modülü yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğesinin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="662e9-236">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="662e9-237">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-237">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="662e9-238">Uygulama olay günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-238">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="662e9-239">Uygulama olay günlüğüne erişmek için Azure portal **sorunları Tanıla ve çöz** dikey penceresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="662e9-239">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="662e9-240">Azure portal uygulama **Hizmetleri**' nde uygulamayı açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-240">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="662e9-241">**Tanıla ve sorunları çöz '** ü seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-241">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="662e9-242">**Tanılama araçları** başlığını seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-242">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="662e9-243">**Destek Araçları**' nın altında, **uygulama olayları** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-243">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="662e9-244">**Kaynak** sütununda *IIS AspNetCoreModule* veya *IIS Aspnetcoremodule v2* girişi tarafından belirtilen en son hatayı inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-244">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="662e9-245">**Sorunları Tanıla ve çöz** dikey penceresini kullanmanın bir alternatifi, uygulama olay günlüğü dosyasını doğrudan [kudu](https://github.com/projectkudu/kudu/wiki)kullanarak incelemektir:</span><span class="sxs-lookup"><span data-stu-id="662e9-245">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="662e9-246">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-246">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-247">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-247">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-248">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-248">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-249">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-249">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-250">**LogFiles** klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-250">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="662e9-251">*EventLog. xml* dosyasının yanındaki kurşun kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-251">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="662e9-252">Günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-252">Examine the log.</span></span> <span data-ttu-id="662e9-253">En son olayları görmek için günlüğün en altına gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-253">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="662e9-254">Uygulamayı kudu konsolunda çalıştırma</span><span class="sxs-lookup"><span data-stu-id="662e9-254">Run the app in the Kudu console</span></span>

<span data-ttu-id="662e9-255">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="662e9-255">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="662e9-256">Bu hatayı saptamak için, uygulamayı [kudu](https://github.com/projectkudu/kudu/wiki) uzaktan yürütme konsolu 'nda çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="662e9-256">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="662e9-257">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-257">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-258">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-258">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-259">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-259">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-260">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-260">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="662e9-261">32 bit (x86) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="662e9-261">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="662e9-262">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="662e9-262">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="662e9-263">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-263">Run the app:</span></span>
   * <span data-ttu-id="662e9-264">Uygulama, [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-264">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="662e9-265">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-265">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="662e9-266">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-266">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="662e9-267">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="662e9-267">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="662e9-268">*ASP.NET Core {VERSION} (x86) çalışma zamanı site uzantısının yüklenmesini gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="662e9-268">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="662e9-269">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="662e9-269">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="662e9-270">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-270">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="662e9-271">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-271">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="662e9-272">64 bit (x64) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="662e9-272">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="662e9-273">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="662e9-273">**Current release**</span></span>

* <span data-ttu-id="662e9-274">Uygulama 64 bit (x64) [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-274">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="662e9-275">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-275">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="662e9-276">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-276">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="662e9-277">Uygulamayı çalıştırın: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="662e9-277">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="662e9-278">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-278">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="662e9-279">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="662e9-279">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="662e9-280">*ASP.NET Core {VERSION} (x64) çalışma zamanı site uzantısını yüklemeyi gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="662e9-280">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="662e9-281">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="662e9-281">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="662e9-282">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-282">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="662e9-283">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-283">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="662e9-284">ASP.NET Core modülü stdout günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-284">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="662e9-285">ASP.NET Core Module stdout günlüğü genellikle uygulama olay günlüğünde bulunmayan yararlı hata iletilerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="662e9-285">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="662e9-286">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-286">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-287">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-287">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="662e9-288">**Sorun kategorisini seçin**altında **Web uygulaması aşağı** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-288">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="662e9-289">**Önerilen çözümler** ' de **stdout günlük yeniden yönlendirmeyi etkinleştirmek**>, **Web. config dosyasını düzenlemek için kudu konsolunu açmak**üzere düğmeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-289">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="662e9-290">Kudu **Tanılama konsolunda**, dosyaları **Wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-290">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="662e9-291">Listenin altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-291">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="662e9-292">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-292">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-293">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="662e9-293">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="662e9-294">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-294">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="662e9-295">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-295">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-296">Azure portalına dönün.</span><span class="sxs-lookup"><span data-stu-id="662e9-296">Return to the Azure portal.</span></span> <span data-ttu-id="662e9-297">**GELIŞTIRME araçları** alanında **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-297">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="662e9-298">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-298">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-299">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-299">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-300">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-300">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-301">**LogFiles** klasörünü seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-301">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="662e9-302">**Değiştirilen** sütunu inceleyin ve son değiştirilme tarihiyle stdout günlüğünü düzenlemek için kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-302">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="662e9-303">Günlük dosyası açıldığında hata görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-303">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="662e9-304">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-304">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="662e9-305">Kudu **Tanılama konsolunda**, *Web. config* dosyasını açığa çıkarmak için **Wwwroot** > yolu **sitesine** dönün.</span><span class="sxs-lookup"><span data-stu-id="662e9-305">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="662e9-306">Kalem simgesini seçerek **Web. config** dosyasını tekrar açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-306">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="662e9-307">`false`için **stdoutLogEnabled** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-307">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="662e9-308">Dosyayı kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-308">Select **Save** to save the file.</span></span>

<span data-ttu-id="662e9-309">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="662e9-309">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-310">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-310">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-311">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-311">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="662e9-312">Yalnızca uygulama başlatma sorunlarını gidermek için stdout günlüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-312">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="662e9-313">Başlangıçtan sonra ASP.NET Core bir uygulamada genel günlüğe kaydetme için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-313">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-314">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-314">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="662e9-315">ASP.NET Core modülü hata ayıklama günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-315">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="662e9-316">ASP.NET Core Module hata ayıklama günlüğü, ASP.NET Core modülünden daha ayrıntılı günlük kaydı sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-316">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="662e9-317">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-317">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-318">Gelişmiş tanılama günlüğünü etkinleştirmek için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="662e9-318">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="662e9-319">Uygulamayı gelişmiş tanılama günlüğü için yapılandırmak üzere [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-319">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="662e9-320">Uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="662e9-320">Redeploy the app.</span></span>
   * <span data-ttu-id="662e9-321">[Gelişmiş tanılama günlüklerinde](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) gösterilen `<handlerSettings>` kudu konsolunu kullanarak canlı uygulamanın *Web. config* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-321">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="662e9-322">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-322">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-323">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-323">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-324">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-324">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="662e9-325">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-325">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="662e9-326">Dosyaları **wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-326">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="662e9-327">*Web. config* dosyasını, kurşun kalem düğmesini seçerek düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-327">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="662e9-328">`<handlerSettings>` bölümünü, [Gelişmiş tanılama günlüklerinde](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-328">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="662e9-329">**Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-329">Select the **Save** button.</span></span>
1. <span data-ttu-id="662e9-330">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-330">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-331">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-331">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-332">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-332">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-333">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-333">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-334">Dosyaları **wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-334">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="662e9-335">*Aspnetcore-Debug. log* dosyası için bir yol sağlamadıysanız dosya listede görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-335">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="662e9-336">Bir yol sağladıysanız, günlük dosyasının konumuna gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-336">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="662e9-337">Dosya adının yanındaki kurşun kalem düğmesiyle günlük dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-337">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="662e9-338">Sorun giderme tamamlandığında hata ayıklama günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-338">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="662e9-339">Gelişmiş hata ayıklama günlüğünü devre dışı bırakmak için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="662e9-339">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="662e9-340">*Web. config* dosyasından `<handlerSettings>` yerel olarak kaldırın ve uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="662e9-340">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="662e9-341">*Web. config* dosyasını düzenlemek ve `<handlerSettings>` bölümünü kaldırmak Için kudu konsolunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-341">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="662e9-342">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-342">Save the file.</span></span>

<span data-ttu-id="662e9-343">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="662e9-343">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-344">Hata ayıklama günlüğünü devre dışı bırakma hatası, uygulama veya sunucu hatasına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-344">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-345">Günlük dosyası boyutunda sınır yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-345">There's no limit on log file size.</span></span> <span data-ttu-id="662e9-346">Yalnızca uygulama başlatma sorunlarını gidermek için hata ayıklama günlüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-346">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="662e9-347">Başlangıçtan sonra ASP.NET Core bir uygulamada genel günlüğe kaydetme için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-347">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-348">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-348">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="662e9-349">Yavaş veya askıda olan uygulama (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-349">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="662e9-350">Bir uygulama bir istek üzerinde yavaş bir şekilde yanıt verdiğinde veya Kilitlenmelerinde, aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="662e9-350">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="662e9-351">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-351">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="662e9-352">Azure Web uygulamasında aralıklı özel durum sorunları veya performans sorunları için döküm yakalamak üzere kilitlenme tanılayıcı site uzantısı 'nı kullanın</span><span class="sxs-lookup"><span data-stu-id="662e9-352">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="662e9-353">İzleme kanatları</span><span class="sxs-lookup"><span data-stu-id="662e9-353">Monitoring blades</span></span>

<span data-ttu-id="662e9-354">İzleme dikey pencereleri, konusunda daha önce açıklanan yöntemlere alternatif bir sorun giderme deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-354">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="662e9-355">Bu kanatlar 500 serisi hataları tanılamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-355">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="662e9-356">ASP.NET Core uzantılarının yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-356">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="662e9-357">Uzantılar yüklü değilse, bunları el ile yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="662e9-357">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="662e9-358">**GELIŞTIRME araçları** dikey penceresinde **Uzantılar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-358">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="662e9-359">**ASP.NET Core uzantıları** listede görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-359">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="662e9-360">Uzantılar yüklü değilse, **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-360">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="662e9-361">Listeden **ASP.NET Core uzantılarını** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-361">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="662e9-362">Yasal koşulları kabul etmek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-362">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="662e9-363">**Uzantı Ekle** dikey penceresinde **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-363">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="662e9-364">Bilgilendirici bir açılan ileti, uzantıların başarıyla yüklenip yüklenmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="662e9-364">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="662e9-365">Stdout günlüğü etkinleştirilmemişse, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-365">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="662e9-366">Azure portal, **GELIŞTIRME araçları** alanındaki **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-366">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="662e9-367">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-367">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-368">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-368">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-369">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-369">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-370">Dosya yolu > **sitesindeki** klasörleri **açın ve listenin** altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-370">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="662e9-371">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-371">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-372">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="662e9-372">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="662e9-373">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-373">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="662e9-374">Tanılama günlüğünü etkinleştirmek için ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-374">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="662e9-375">Azure portal **tanılama günlükleri** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-375">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="662e9-376">**Uygulama günlüğü (dosya sistemi)** ve **ayrıntılı hata iletileri**için **bir anahtar seçin** .</span><span class="sxs-lookup"><span data-stu-id="662e9-376">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="662e9-377">Dikey pencerenin üst kısmındaki **Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-377">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="662e9-378">Başarısız istek izlemeyi, başarısız Istek olayı arabelleğe alma (FREB) günlüğü olarak da bilinen bir şekilde eklemek için **,** **başarısız istek izleme**anahtarını seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-378">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="662e9-379">Portalda **tanılama günlükleri** dikey penceresinde hemen listelenen **günlük akışı** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-379">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="662e9-380">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-380">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-381">Günlük akışı verileri içinde hatanın nedeni belirtilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-381">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="662e9-382">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bıraktığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-382">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="662e9-383">Başarısız istek izleme günlüklerini görüntülemek için (FREB günlükleri):</span><span class="sxs-lookup"><span data-stu-id="662e9-383">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="662e9-384">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-384">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="662e9-385">Kenar çubuğunun **Destek Araçları** alanından **başarısız istek izleme günlüklerini** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-385">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="662e9-386">[Azure App Service konusundaki Web uygulamaları için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) ve [Azure 'Daki Web Apps Için uygulama performansı SSS](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) bölümündeki başarısız istek izlemeleri bölümüne bakın: daha fazla bilgi için nasıl yaparım? başarısız istek izlemeyi açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-386">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="662e9-387">Daha fazla bilgi için bkz. [Azure App Service Web Apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="662e9-387">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-388">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-388">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-389">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-389">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="662e9-390">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-390">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-391">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-391">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="662e9-392">IIS 'de sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-392">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="662e9-393">Uygulama olay günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-393">Application Event Log (IIS)</span></span>

<span data-ttu-id="662e9-394">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="662e9-394">Access the Application Event Log:</span></span>

1. <span data-ttu-id="662e9-395">Başlat menüsünü açın, *Olay Görüntüleyicisi*araması yapın ve **Olay Görüntüleyicisi** uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-395">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="662e9-396">**Olay Görüntüleyicisi**, **Windows günlükleri** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-396">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="662e9-397">Uygulama olay günlüğünü açmak için **uygulama** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-397">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="662e9-398">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-398">Search for errors associated with the failing app.</span></span> <span data-ttu-id="662e9-399">Hataların, *kaynak* sütununda *IIS aspnetcore modülünün* veya *IIS Express aspnetcore modülünün* bir değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="662e9-399">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="662e9-400">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="662e9-400">Run the app at a command prompt</span></span>

<span data-ttu-id="662e9-401">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="662e9-401">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="662e9-402">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="662e9-402">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="662e9-403">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="662e9-403">Framework-dependent deployment</span></span>

<span data-ttu-id="662e9-404">Uygulama, [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-404">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="662e9-405">Bir komut isteminde, dağıtım klasörüne gidin ve uygulamanın derlemesini *DotNet. exe*ile yürüterek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-405">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="662e9-406">Aşağıdaki komutta, \<assembly_name >: `dotnet .\<assembly_name>.dll`için uygulama derlemesinin adını yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="662e9-406">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="662e9-407">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-407">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="662e9-408">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="662e9-408">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="662e9-409">Varsayılan konak ve gönderi kullanarak `http://localhost:5000/`bir istek yapın.</span><span class="sxs-lookup"><span data-stu-id="662e9-409">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="662e9-410">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="662e9-410">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="662e9-411">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="662e9-411">Self-contained deployment</span></span>

<span data-ttu-id="662e9-412">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-412">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="662e9-413">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-413">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="662e9-414">Aşağıdaki komutta, \<assembly_name >: `<assembly_name>.exe`için uygulama derlemesinin adını yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="662e9-414">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="662e9-415">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-415">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="662e9-416">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="662e9-416">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="662e9-417">Varsayılan konak ve gönderi kullanarak `http://localhost:5000/`bir istek yapın.</span><span class="sxs-lookup"><span data-stu-id="662e9-417">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="662e9-418">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="662e9-418">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="662e9-419">ASP.NET Core Module stdout günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-419">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="662e9-420">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-420">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-421">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-421">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="662e9-422">*Günlükler* klasörü yoksa, klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-422">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="662e9-423">MSBuild 'in dağıtımdaki *Günlükler* klasörünü otomatik olarak oluşturmak üzere nasıl etkinleştirileceği hakkında yönergeler için, bkz. [Dizin yapısı](xref:host-and-deploy/directory-structure) konusu.</span><span class="sxs-lookup"><span data-stu-id="662e9-423">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="662e9-424">*Web. config* dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-424">Edit the *web.config* file.</span></span> <span data-ttu-id="662e9-425">**StdoutLogEnabled** öğesini `true` olarak ayarlayın ve **stdoutLogFile** yolunu *Günlükler* klasörünü işaret etmek üzere değiştirin (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="662e9-425">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="662e9-426">yoldaki `stdout` günlük dosyası adı önekidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-426">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="662e9-427">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-427">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="662e9-428">Dosya adı ön eki olarak `stdout` kullanarak, tipik bir günlük dosyası, *stdout_20180205184032_5412. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-428">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="662e9-429">Uygulama havuzunuzun kimliğinin *Günlükler* klasörü için yazma izinlerine sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-429">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="662e9-430">Güncelleştirilmiş *Web. config* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-430">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="662e9-431">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-431">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-432">*Günlükler* klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-432">Navigate to the *logs* folder.</span></span> <span data-ttu-id="662e9-433">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-433">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="662e9-434">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-434">Study the log for errors.</span></span>

<span data-ttu-id="662e9-435">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-435">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="662e9-436">*Web. config* dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-436">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-437">`false`için **stdoutLogEnabled** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-437">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="662e9-438">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-438">Save the file.</span></span>

<span data-ttu-id="662e9-439">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="662e9-439">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-440">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-440">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-441">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-441">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="662e9-442">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-442">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-443">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-443">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="662e9-444">ASP.NET Core modülü hata ayıklama günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-444">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="662e9-445">ASP.NET Core modülü hata ayıklama günlüğünü etkinleştirmek için aşağıdaki işleyici ayarlarını uygulamanın *Web. config* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-445">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="662e9-446">Günlüğü için belirtilen yolun var olduğundan ve uygulama havuzu kimliğinin konumuna yazma izinlerine sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-446">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="662e9-447">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="662e9-447">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="662e9-448">Geliştirici özel durumu sayfasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="662e9-448">Enable the Developer Exception Page</span></span>

<span data-ttu-id="662e9-449">`ASPNETCORE_ENVIRONMENT` ortam değişkeni, uygulamayı geliştirme ortamında çalıştırmak için [Web. config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) .</span><span class="sxs-lookup"><span data-stu-id="662e9-449">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="662e9-450">Ortam, ana bilgisayar tasarımcısında `UseEnvironment` tarafından uygulama başlangıcında geçersiz kılınmadığı sürece, ortam değişkenini ayarlamak, uygulama çalıştırıldığında [Geliştirici özel durum sayfasının](xref:fundamentals/error-handling) görünmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="662e9-450">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

<span data-ttu-id="662e9-451">`ASPNETCORE_ENVIRONMENT` için ortam değişkenini ayarlamak yalnızca Internet 'e açık olmayan hazırlama ve test etme sunucularında kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-451">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="662e9-452">Sorun giderme işleminden sonra *Web. config* dosyasından ortam değişkenini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-452">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="662e9-453">*Web. config*'de ortam değişkenlerini ayarlama hakkında daha fazla bilgi Için, [Aspnetcore 'un EnvironmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="662e9-453">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="662e9-454">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="662e9-454">Obtain data from an app</span></span>

<span data-ttu-id="662e9-455">Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin.</span><span class="sxs-lookup"><span data-stu-id="662e9-455">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="662e9-456">Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="662e9-456">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="662e9-457">Yavaş veya askıda olan uygulama (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-457">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="662e9-458">*Kilitlenme dökümü* , sistem belleğinin bir anlık görüntüsüdür ve uygulama kilitlenmesinin, başlatma hatasının veya yavaş uygulamanın nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-458">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="662e9-459">Uygulama kilitleniyor veya bir özel durumla karşılaşırsa</span><span class="sxs-lookup"><span data-stu-id="662e9-459">App crashes or encounters an exception</span></span>

<span data-ttu-id="662e9-460">Windows Hata Bildirimi bir döküm edinin ve çözümleyin [(WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="662e9-460">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="662e9-461">Kilitlenme döküm dosyalarını `c:\dumps`tutmak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-461">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="662e9-462">Uygulama havuzunun klasöre yazma erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-462">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="662e9-463">[Enabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-463">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="662e9-464">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-464">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="662e9-465">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-465">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="662e9-466">Uygulamayı kilitlenmenin oluşmasına neden olan koşullar altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-466">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="662e9-467">Kilitlenme gerçekleştirildikten sonra, [Disabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-467">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="662e9-468">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-468">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="662e9-469">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-469">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="662e9-470">Uygulama kilitlenmeleri ve döküm koleksiyonu tamamlandıktan sonra, uygulamanın normal olarak sonlandırılmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-470">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="662e9-471">PowerShell betiği, WER 'i uygulama başına en fazla beş döküm toplayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="662e9-471">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-472">Kilitlenme dökümleri büyük miktarda disk alanı kaplar (her birine kadar çok gigabayt kadar).</span><span class="sxs-lookup"><span data-stu-id="662e9-472">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="662e9-473">Uygulama askıda kalıyor, başlatma sırasında başarısız oluyor veya normal şekilde çalışıyor</span><span class="sxs-lookup"><span data-stu-id="662e9-473">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="662e9-474">Bir uygulama *askıda* kaldığında (yanıt vermeyi keser ancak kilitlenmez), başlatma sırasında başarısız olur veya normal şekilde çalışır. [Kullanıcı modu döküm dosyaları:](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçmek üzere en iyi aracı seçme.</span><span class="sxs-lookup"><span data-stu-id="662e9-474">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="662e9-475">Dökümü çözümle</span><span class="sxs-lookup"><span data-stu-id="662e9-475">Analyze the dump</span></span>

<span data-ttu-id="662e9-476">Bir döküm çeşitli yaklaşımlar kullanılarak analiz edilebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-476">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="662e9-477">Daha fazla bilgi için bkz. [Kullanıcı modu döküm dosyasını çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="662e9-477">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="662e9-478">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="662e9-478">Clear package caches</span></span>

<span data-ttu-id="662e9-479">Çalışan bir uygulama, geliştirme makinesindeki .NET Core SDK yükseltmeden veya uygulama içindeki paket sürümlerini değiştirirken hemen başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-479">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="662e9-480">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-480">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="662e9-481">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="662e9-481">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="662e9-482">*Bin* ve *obj* klasörlerini silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-482">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="662e9-483">Bir komut kabuğundan [DotNet NuGet yerelleri, Tümünü Temizle](/dotnet/core/tools/dotnet-nuget-locals) ' i yürüterek paket önbelleklerini temizleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-483">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="662e9-484">Paket önbelleklerini Temizleme, [NuGet. exe](https://www.nuget.org/downloads) aracı ile de gerçekleştirilebilir ve komut `nuget locals all -clear`yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-484">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="662e9-485">*NuGet. exe* , Windows masaüstü işletim sistemiyle birlikte paketlenmiş bir yüklemedir ve [NuGet Web sitesinden](https://www.nuget.org/downloads)ayrı olarak alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-485">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="662e9-486">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-486">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="662e9-487">Uygulamayı yeniden dağıtmadan önce sunucusundaki dağıtım klasöründeki tüm dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-487">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="662e9-488">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="662e9-488">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="662e9-489">Azure belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-489">Azure documentation</span></span>

* [<span data-ttu-id="662e9-490">ASP.NET Core için Application Insights</span><span class="sxs-lookup"><span data-stu-id="662e9-490">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="662e9-491">Visual Studio 'Yu kullanarak Azure App Service Web uygulamasının sorunlarını giderme bölümünde uzaktan hata ayıklama Web Apps bölümü</span><span class="sxs-lookup"><span data-stu-id="662e9-491">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="662e9-492">Azure App Service tanılamada genel bakış</span><span class="sxs-lookup"><span data-stu-id="662e9-492">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="662e9-493">Nasıl Yapılır: Azure App Service’te Uygulamaları İzleme</span><span class="sxs-lookup"><span data-stu-id="662e9-493">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="662e9-494">Visual Studio 'Yu kullanarak Azure App Service bir Web uygulamasının sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-494">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="662e9-495">Azure Web uygulamalarınızda "502 hatalı Ağ Geçidi" ve "503 hizmeti kullanılamıyor" HTTP hatalarında sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-495">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="662e9-496">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-496">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="662e9-497">Azure 'da Web Apps için uygulama performansı SSS</span><span class="sxs-lookup"><span data-stu-id="662e9-497">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="662e9-498">Azure Web uygulaması korumalı alanı (App Service çalışma zamanı yürütme sınırlamaları)</span><span class="sxs-lookup"><span data-stu-id="662e9-498">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="662e9-499">Azure Cuma: Azure App Service tanılama ve sorun giderme deneyimi (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="662e9-499">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="662e9-500">Visual Studio belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-500">Visual Studio documentation</span></span>

* [<span data-ttu-id="662e9-501">Visual Studio 2017 ' de Azure 'da IIS 'de uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="662e9-501">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="662e9-502">Visual Studio 2017 ' de uzak IIS bilgisayarında uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="662e9-502">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="662e9-503">Visual Studio kullanarak hata ayıklamayı öğrenin</span><span class="sxs-lookup"><span data-stu-id="662e9-503">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="662e9-504">Visual Studio Code belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-504">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="662e9-505">Visual Studio Code ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="662e9-505">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="662e9-506">Bu makalede, bir uygulama Azure App Service veya IIS 'ye dağıtıldığında hataların nasıl tanılanacağı hakkında genel uygulama başlatma hataları ve yönergeleri hakkında bilgi verilmektedir:</span><span class="sxs-lookup"><span data-stu-id="662e9-506">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="662e9-507">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="662e9-507">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="662e9-508">Ortak Başlangıç HTTP durum kodu senaryolarını açıklar.</span><span class="sxs-lookup"><span data-stu-id="662e9-508">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="662e9-509">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-509">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="662e9-510">Azure App Service dağıtılan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-510">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="662e9-511">IIS üzerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-511">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="662e9-512">IIS 'ye dağıtılan veya IIS Express yerel olarak çalışan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-512">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="662e9-513">Bu kılavuz hem Windows Server hem de Windows masaüstü dağıtımları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-513">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="662e9-514">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="662e9-514">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="662e9-515">Önemli güncelleştirmeler gerçekleştirirken veya paket sürümlerini değiştirirken ne yapmanız gerektiğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="662e9-515">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="662e9-516">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="662e9-516">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="662e9-517">Ek sorun giderme konularını listeler.</span><span class="sxs-lookup"><span data-stu-id="662e9-517">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="662e9-518">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="662e9-518">App startup errors</span></span>

<span data-ttu-id="662e9-519">Visual Studio 'da bir ASP.NET Core projesi, hata ayıklama sırasında [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) barındırmak için varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="662e9-519">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="662e9-520">*502,5-Işlem hatası* veya yerel olarak hata ayıklarken oluşan *500,30-başlatma hatası* , bu konudaki öneri kullanılarak tanılanabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-520">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="662e9-521">403,14 yasak</span><span class="sxs-lookup"><span data-stu-id="662e9-521">403.14 Forbidden</span></span>

<span data-ttu-id="662e9-522">Uygulama başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-522">The app fails to start.</span></span> <span data-ttu-id="662e9-523">Aşağıdaki hata günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="662e9-523">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="662e9-524">Hata genellikle barındırma sisteminde, aşağıdaki senaryolardan birini içeren bozuk bir dağıtım nedeniyle oluşur:</span><span class="sxs-lookup"><span data-stu-id="662e9-524">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="662e9-525">Uygulama, barındırma sisteminde yanlış klasöre dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-525">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="662e9-526">Dağıtım işlemi, uygulamanın tüm dosyalarını ve klasörlerini barındırma sistemindeki dağıtım klasörüne taşıyamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-526">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="662e9-527">*Web. config* dosyası dağıtımda yok veya *Web. config* dosyası içerikleri hatalı biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="662e9-527">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="662e9-528">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="662e9-528">Perform the following steps:</span></span>

1. <span data-ttu-id="662e9-529">Tüm dosya ve klasörleri barındırma sistemindeki dağıtım klasöründen silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-529">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="662e9-530">Visual Studio, PowerShell veya el ile dağıtım gibi normal dağıtım yönteminizi kullanarak, uygulamanın *Yayımlama* klasörünün içeriğini barındırma sistemine yeniden dağıtın:</span><span class="sxs-lookup"><span data-stu-id="662e9-530">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="662e9-531">*Web. config* dosyasının dağıtımda mevcut olduğunu ve içeriğinin doğru olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-531">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="662e9-532">Azure App Service barındırırken, uygulamanın `D:\home\site\wwwroot` klasörüne dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-532">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="662e9-533">Uygulama IIS tarafından barındırılıyorsa, uygulamanın **IIS yöneticisinin** **temel ayarlarında**gösterilen IIS **fiziksel yoluna** dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-533">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="662e9-534">Barındırma sistemindeki dağıtımı projenin *Yayımla* klasörünün içeriğiyle karşılaştırarak uygulamanın tüm dosya ve klasörlerinin dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-534">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="662e9-535">Yayımlanan ASP.NET Core uygulamasının düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="662e9-535">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="662e9-536">*Web. config* dosyası hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="662e9-536">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="662e9-537">500 İç Sunucu Hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-537">500 Internal Server Error</span></span>

<span data-ttu-id="662e9-538">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="662e9-538">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="662e9-539">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-539">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="662e9-540">Yanıtta içerik yok olabilir veya Yanıt, tarayıcıda *500 Iç sunucu hatası* olarak görünebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-540">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="662e9-541">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="662e9-541">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="662e9-542">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-542">From the server's perspective, that's correct.</span></span> <span data-ttu-id="662e9-543">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-543">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="662e9-544">Uygulamayı sunucuda bir komut isteminde çalıştırın veya sorunu gidermek için ASP.NET Core modülü stdout günlüğünü etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="662e9-544">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="662e9-545">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-545">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="662e9-546">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-546">The worker process fails.</span></span> <span data-ttu-id="662e9-547">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-547">The app doesn't start.</span></span>

<span data-ttu-id="662e9-548">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) .NET Core CLR 'yi bulamıyor ve işlem içi istek işleyicisini (*aspnetcorev2_inprocess. dll*) bulamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-548">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="662e9-549">Kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="662e9-549">Check that:</span></span>

* <span data-ttu-id="662e9-550">Uygulama [Microsoft. AspNetCore. Server. IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini ya da [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)'i hedefler.</span><span class="sxs-lookup"><span data-stu-id="662e9-550">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="662e9-551">ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.</span><span class="sxs-lookup"><span data-stu-id="662e9-551">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="662e9-552">500.0 giden işlem işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-552">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="662e9-553">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-553">The worker process fails.</span></span> <span data-ttu-id="662e9-554">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-554">The app doesn't start.</span></span>

<span data-ttu-id="662e9-555">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) işlem dışı barındırma isteği işleyicisini bulamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-555">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="662e9-556">*Aspnetcorev2_outofprocess. dll* ' nin *aspnetcorev2. dll*' nin yanındaki bir alt klasörde bulunduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-556">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="662e9-557">502.5 işlem hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-557">502.5 Process Failure</span></span>

<span data-ttu-id="662e9-558">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-558">The worker process fails.</span></span> <span data-ttu-id="662e9-559">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-559">The app doesn't start.</span></span>

<span data-ttu-id="662e9-560">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) çalışan işlemini başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-560">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="662e9-561">İşlem başlatma hatasının nedeni genellikle uygulama olay günlüğündeki girişlerden ve ASP.NET Core modülü stdout günlüğünde belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-561">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="662e9-562">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="662e9-562">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="662e9-563">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-563">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="662e9-564">*Paylaşılan çerçeve* , makinede yüklü olan ve `Microsoft.AspNetCore.App`gibi bir metapackage tarafından başvurulan derleme ( *. dll* dosyaları) kümesidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-564">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="662e9-565">Metapackage başvurusu, gerekli en düşük sürümü belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-565">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="662e9-566">Daha fazla bilgi için bkz. [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="662e9-566">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="662e9-567">Bir barındırma veya uygulamanın yanlış yapılandırılması, çalışan işleminin başarısız olmasına neden olduğunda, *502,5 Işlem hata* hatası sayfası döndürülür:</span><span class="sxs-lookup"><span data-stu-id="662e9-567">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="662e9-568">Uygulama (hata kodu: '0x800700c1') başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-568">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="662e9-569">Uygulamanın derlemesi ( *. dll*) yüklenemediğinden uygulama başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-569">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="662e9-570">W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-570">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="662e9-571">Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="662e9-571">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="662e9-572">IIS yöneticisinin **uygulama havuzlarında**uygulama havuzunu seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-572">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="662e9-573">**Eylemler** panelinde **uygulama havuzunu Düzenle** altında **Gelişmiş ayarlar** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-573">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="662e9-574">**Enable 32 bit uygulamalarını**ayarla:</span><span class="sxs-lookup"><span data-stu-id="662e9-574">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="662e9-575">32-bit (x86) bir uygulama dağıtıyorsanız, değeri `True`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-575">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="662e9-576">64 bit (x64) uygulaması dağıtıyorsanız, değeri `False`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-576">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="662e9-577">Proje dosyasındaki `<Platform>` MSBuild özelliği ile uygulamanın yayınlanan bit durumuyla ilgili bir çakışma olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-577">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="662e9-578">Bağlantı sıfırlama</span><span class="sxs-lookup"><span data-stu-id="662e9-578">Connection reset</span></span>

<span data-ttu-id="662e9-579">Üstbilgiler gönderildikten sonra bir hata oluşursa, bir hata oluştuğunda sunucunun **500 Iç sunucu hatası** gönderebilmesi için çok geç olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-579">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="662e9-580">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="662e9-580">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="662e9-581">Bu tür bir hata, istemcide bir *bağlantı sıfırlama* hatası olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-581">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="662e9-582">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataların giderilmesine yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-582">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="662e9-583">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="662e9-583">Default startup limits</span></span>

<span data-ttu-id="662e9-584">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) varsayılan bir *StartupTimeLimit* 120 saniye ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-584">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="662e9-585">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-585">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="662e9-586">Modülü yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğesinin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="662e9-586">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="662e9-587">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-587">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="662e9-588">Uygulama olay günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-588">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="662e9-589">Uygulama olay günlüğüne erişmek için Azure portal **sorunları Tanıla ve çöz** dikey penceresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="662e9-589">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="662e9-590">Azure portal uygulama **Hizmetleri**' nde uygulamayı açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-590">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="662e9-591">**Tanıla ve sorunları çöz '** ü seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-591">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="662e9-592">**Tanılama araçları** başlığını seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-592">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="662e9-593">**Destek Araçları**' nın altında, **uygulama olayları** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-593">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="662e9-594">**Kaynak** sütununda *IIS AspNetCoreModule* veya *IIS Aspnetcoremodule v2* girişi tarafından belirtilen en son hatayı inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-594">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="662e9-595">**Sorunları Tanıla ve çöz** dikey penceresini kullanmanın bir alternatifi, uygulama olay günlüğü dosyasını doğrudan [kudu](https://github.com/projectkudu/kudu/wiki)kullanarak incelemektir:</span><span class="sxs-lookup"><span data-stu-id="662e9-595">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="662e9-596">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-596">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-597">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-597">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-598">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-598">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-599">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-599">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-600">**LogFiles** klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-600">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="662e9-601">*EventLog. xml* dosyasının yanındaki kurşun kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-601">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="662e9-602">Günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-602">Examine the log.</span></span> <span data-ttu-id="662e9-603">En son olayları görmek için günlüğün en altına gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-603">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="662e9-604">Uygulamayı kudu konsolunda çalıştırma</span><span class="sxs-lookup"><span data-stu-id="662e9-604">Run the app in the Kudu console</span></span>

<span data-ttu-id="662e9-605">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="662e9-605">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="662e9-606">Bu hatayı saptamak için, uygulamayı [kudu](https://github.com/projectkudu/kudu/wiki) uzaktan yürütme konsolu 'nda çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="662e9-606">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="662e9-607">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-607">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-608">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-608">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-609">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-609">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-610">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-610">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="662e9-611">32 bit (x86) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="662e9-611">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="662e9-612">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="662e9-612">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="662e9-613">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-613">Run the app:</span></span>
   * <span data-ttu-id="662e9-614">Uygulama, [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-614">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="662e9-615">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-615">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="662e9-616">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-616">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="662e9-617">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="662e9-617">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="662e9-618">*ASP.NET Core {VERSION} (x86) çalışma zamanı site uzantısının yüklenmesini gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="662e9-618">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="662e9-619">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="662e9-619">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="662e9-620">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-620">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="662e9-621">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-621">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="662e9-622">64 bit (x64) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="662e9-622">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="662e9-623">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="662e9-623">**Current release**</span></span>

* <span data-ttu-id="662e9-624">Uygulama 64 bit (x64) [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-624">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="662e9-625">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-625">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="662e9-626">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-626">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="662e9-627">Uygulamayı çalıştırın: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="662e9-627">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="662e9-628">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-628">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="662e9-629">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="662e9-629">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="662e9-630">*ASP.NET Core {VERSION} (x64) çalışma zamanı site uzantısını yüklemeyi gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="662e9-630">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="662e9-631">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="662e9-631">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="662e9-632">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-632">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="662e9-633">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-633">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="662e9-634">ASP.NET Core modülü stdout günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-634">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="662e9-635">ASP.NET Core Module stdout günlüğü genellikle uygulama olay günlüğünde bulunmayan yararlı hata iletilerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="662e9-635">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="662e9-636">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-636">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-637">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-637">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="662e9-638">**Sorun kategorisini seçin**altında **Web uygulaması aşağı** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-638">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="662e9-639">**Önerilen çözümler** ' de **stdout günlük yeniden yönlendirmeyi etkinleştirmek**>, **Web. config dosyasını düzenlemek için kudu konsolunu açmak**üzere düğmeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-639">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="662e9-640">Kudu **Tanılama konsolunda**, dosyaları **Wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-640">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="662e9-641">Listenin altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-641">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="662e9-642">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-642">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-643">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="662e9-643">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="662e9-644">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-644">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="662e9-645">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-645">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-646">Azure portalına dönün.</span><span class="sxs-lookup"><span data-stu-id="662e9-646">Return to the Azure portal.</span></span> <span data-ttu-id="662e9-647">**GELIŞTIRME araçları** alanında **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-647">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="662e9-648">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-648">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-649">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-649">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-650">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-650">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-651">**LogFiles** klasörünü seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-651">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="662e9-652">**Değiştirilen** sütunu inceleyin ve son değiştirilme tarihiyle stdout günlüğünü düzenlemek için kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-652">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="662e9-653">Günlük dosyası açıldığında hata görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-653">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="662e9-654">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-654">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="662e9-655">Kudu **Tanılama konsolunda**, *Web. config* dosyasını açığa çıkarmak için **Wwwroot** > yolu **sitesine** dönün.</span><span class="sxs-lookup"><span data-stu-id="662e9-655">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="662e9-656">Kalem simgesini seçerek **Web. config** dosyasını tekrar açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-656">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="662e9-657">`false`için **stdoutLogEnabled** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-657">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="662e9-658">Dosyayı kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-658">Select **Save** to save the file.</span></span>

<span data-ttu-id="662e9-659">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="662e9-659">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-660">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-660">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-661">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-661">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="662e9-662">Yalnızca uygulama başlatma sorunlarını gidermek için stdout günlüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-662">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="662e9-663">Başlangıçtan sonra ASP.NET Core bir uygulamada genel günlüğe kaydetme için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-663">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-664">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-664">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="662e9-665">ASP.NET Core modülü hata ayıklama günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-665">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="662e9-666">ASP.NET Core Module hata ayıklama günlüğü, ASP.NET Core modülünden daha ayrıntılı günlük kaydı sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-666">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="662e9-667">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-667">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-668">Gelişmiş tanılama günlüğünü etkinleştirmek için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="662e9-668">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="662e9-669">Uygulamayı gelişmiş tanılama günlüğü için yapılandırmak üzere [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-669">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="662e9-670">Uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="662e9-670">Redeploy the app.</span></span>
   * <span data-ttu-id="662e9-671">[Gelişmiş tanılama günlüklerinde](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) gösterilen `<handlerSettings>` kudu konsolunu kullanarak canlı uygulamanın *Web. config* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-671">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="662e9-672">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-672">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-673">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-673">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-674">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-674">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="662e9-675">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-675">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="662e9-676">Dosyaları **wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-676">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="662e9-677">*Web. config* dosyasını, kurşun kalem düğmesini seçerek düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-677">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="662e9-678">`<handlerSettings>` bölümünü, [Gelişmiş tanılama günlüklerinde](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-678">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="662e9-679">**Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-679">Select the **Save** button.</span></span>
1. <span data-ttu-id="662e9-680">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-680">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-681">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-681">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-682">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-682">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-683">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-683">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-684">Dosyaları **wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-684">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="662e9-685">*Aspnetcore-Debug. log* dosyası için bir yol sağlamadıysanız dosya listede görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-685">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="662e9-686">Bir yol sağladıysanız, günlük dosyasının konumuna gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-686">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="662e9-687">Dosya adının yanındaki kurşun kalem düğmesiyle günlük dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-687">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="662e9-688">Sorun giderme tamamlandığında hata ayıklama günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-688">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="662e9-689">Gelişmiş hata ayıklama günlüğünü devre dışı bırakmak için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="662e9-689">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="662e9-690">*Web. config* dosyasından `<handlerSettings>` yerel olarak kaldırın ve uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="662e9-690">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="662e9-691">*Web. config* dosyasını düzenlemek ve `<handlerSettings>` bölümünü kaldırmak Için kudu konsolunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-691">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="662e9-692">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-692">Save the file.</span></span>

<span data-ttu-id="662e9-693">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="662e9-693">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-694">Hata ayıklama günlüğünü devre dışı bırakma hatası, uygulama veya sunucu hatasına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-694">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-695">Günlük dosyası boyutunda sınır yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-695">There's no limit on log file size.</span></span> <span data-ttu-id="662e9-696">Yalnızca uygulama başlatma sorunlarını gidermek için hata ayıklama günlüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-696">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="662e9-697">Başlangıçtan sonra ASP.NET Core bir uygulamada genel günlüğe kaydetme için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-697">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-698">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-698">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="662e9-699">Yavaş veya askıda olan uygulama (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-699">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="662e9-700">Bir uygulama bir istek üzerinde yavaş bir şekilde yanıt verdiğinde veya Kilitlenmelerinde, aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="662e9-700">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="662e9-701">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-701">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="662e9-702">Azure Web uygulamasında aralıklı özel durum sorunları veya performans sorunları için döküm yakalamak üzere kilitlenme tanılayıcı site uzantısı 'nı kullanın</span><span class="sxs-lookup"><span data-stu-id="662e9-702">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="662e9-703">İzleme kanatları</span><span class="sxs-lookup"><span data-stu-id="662e9-703">Monitoring blades</span></span>

<span data-ttu-id="662e9-704">İzleme dikey pencereleri, konusunda daha önce açıklanan yöntemlere alternatif bir sorun giderme deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-704">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="662e9-705">Bu kanatlar 500 serisi hataları tanılamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-705">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="662e9-706">ASP.NET Core uzantılarının yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-706">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="662e9-707">Uzantılar yüklü değilse, bunları el ile yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="662e9-707">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="662e9-708">**GELIŞTIRME araçları** dikey penceresinde **Uzantılar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-708">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="662e9-709">**ASP.NET Core uzantıları** listede görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-709">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="662e9-710">Uzantılar yüklü değilse, **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-710">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="662e9-711">Listeden **ASP.NET Core uzantılarını** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-711">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="662e9-712">Yasal koşulları kabul etmek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-712">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="662e9-713">**Uzantı Ekle** dikey penceresinde **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-713">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="662e9-714">Bilgilendirici bir açılan ileti, uzantıların başarıyla yüklenip yüklenmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="662e9-714">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="662e9-715">Stdout günlüğü etkinleştirilmemişse, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-715">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="662e9-716">Azure portal, **GELIŞTIRME araçları** alanındaki **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-716">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="662e9-717">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-717">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-718">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-718">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-719">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-719">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-720">Dosya yolu > **sitesindeki** klasörleri **açın ve listenin** altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-720">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="662e9-721">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-721">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-722">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="662e9-722">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="662e9-723">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-723">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="662e9-724">Tanılama günlüğünü etkinleştirmek için ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-724">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="662e9-725">Azure portal **tanılama günlükleri** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-725">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="662e9-726">**Uygulama günlüğü (dosya sistemi)** ve **ayrıntılı hata iletileri**için **bir anahtar seçin** .</span><span class="sxs-lookup"><span data-stu-id="662e9-726">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="662e9-727">Dikey pencerenin üst kısmındaki **Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-727">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="662e9-728">Başarısız istek izlemeyi, başarısız Istek olayı arabelleğe alma (FREB) günlüğü olarak da bilinen bir şekilde eklemek için **,** **başarısız istek izleme**anahtarını seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-728">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="662e9-729">Portalda **tanılama günlükleri** dikey penceresinde hemen listelenen **günlük akışı** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-729">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="662e9-730">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-730">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-731">Günlük akışı verileri içinde hatanın nedeni belirtilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-731">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="662e9-732">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bıraktığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-732">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="662e9-733">Başarısız istek izleme günlüklerini görüntülemek için (FREB günlükleri):</span><span class="sxs-lookup"><span data-stu-id="662e9-733">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="662e9-734">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-734">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="662e9-735">Kenar çubuğunun **Destek Araçları** alanından **başarısız istek izleme günlüklerini** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-735">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="662e9-736">[Azure App Service konusundaki Web uygulamaları için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) ve [Azure 'Daki Web Apps Için uygulama performansı SSS](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) bölümündeki başarısız istek izlemeleri bölümüne bakın: daha fazla bilgi için nasıl yaparım? başarısız istek izlemeyi açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-736">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="662e9-737">Daha fazla bilgi için bkz. [Azure App Service Web Apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="662e9-737">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-738">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-738">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-739">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-739">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="662e9-740">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-740">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-741">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-741">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="662e9-742">IIS 'de sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-742">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="662e9-743">Uygulama olay günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-743">Application Event Log (IIS)</span></span>

<span data-ttu-id="662e9-744">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="662e9-744">Access the Application Event Log:</span></span>

1. <span data-ttu-id="662e9-745">Başlat menüsünü açın, *Olay Görüntüleyicisi*araması yapın ve **Olay Görüntüleyicisi** uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-745">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="662e9-746">**Olay Görüntüleyicisi**, **Windows günlükleri** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-746">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="662e9-747">Uygulama olay günlüğünü açmak için **uygulama** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-747">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="662e9-748">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-748">Search for errors associated with the failing app.</span></span> <span data-ttu-id="662e9-749">Hataların, *kaynak* sütununda *IIS aspnetcore modülünün* veya *IIS Express aspnetcore modülünün* bir değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="662e9-749">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="662e9-750">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="662e9-750">Run the app at a command prompt</span></span>

<span data-ttu-id="662e9-751">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="662e9-751">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="662e9-752">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="662e9-752">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="662e9-753">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="662e9-753">Framework-dependent deployment</span></span>

<span data-ttu-id="662e9-754">Uygulama, [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-754">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="662e9-755">Bir komut isteminde, dağıtım klasörüne gidin ve uygulamanın derlemesini *DotNet. exe*ile yürüterek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-755">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="662e9-756">Aşağıdaki komutta, \<assembly_name >: `dotnet .\<assembly_name>.dll`için uygulama derlemesinin adını yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="662e9-756">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="662e9-757">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-757">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="662e9-758">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="662e9-758">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="662e9-759">Varsayılan konak ve gönderi kullanarak `http://localhost:5000/`bir istek yapın.</span><span class="sxs-lookup"><span data-stu-id="662e9-759">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="662e9-760">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="662e9-760">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="662e9-761">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="662e9-761">Self-contained deployment</span></span>

<span data-ttu-id="662e9-762">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-762">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="662e9-763">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-763">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="662e9-764">Aşağıdaki komutta, \<assembly_name >: `<assembly_name>.exe`için uygulama derlemesinin adını yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="662e9-764">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="662e9-765">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-765">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="662e9-766">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="662e9-766">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="662e9-767">Varsayılan konak ve gönderi kullanarak `http://localhost:5000/`bir istek yapın.</span><span class="sxs-lookup"><span data-stu-id="662e9-767">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="662e9-768">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="662e9-768">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="662e9-769">ASP.NET Core Module stdout günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-769">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="662e9-770">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-770">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-771">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-771">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="662e9-772">*Günlükler* klasörü yoksa, klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-772">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="662e9-773">MSBuild 'in dağıtımdaki *Günlükler* klasörünü otomatik olarak oluşturmak üzere nasıl etkinleştirileceği hakkında yönergeler için, bkz. [Dizin yapısı](xref:host-and-deploy/directory-structure) konusu.</span><span class="sxs-lookup"><span data-stu-id="662e9-773">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="662e9-774">*Web. config* dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-774">Edit the *web.config* file.</span></span> <span data-ttu-id="662e9-775">**StdoutLogEnabled** öğesini `true` olarak ayarlayın ve **stdoutLogFile** yolunu *Günlükler* klasörünü işaret etmek üzere değiştirin (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="662e9-775">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="662e9-776">yoldaki `stdout` günlük dosyası adı önekidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-776">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="662e9-777">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-777">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="662e9-778">Dosya adı ön eki olarak `stdout` kullanarak, tipik bir günlük dosyası, *stdout_20180205184032_5412. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-778">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="662e9-779">Uygulama havuzunuzun kimliğinin *Günlükler* klasörü için yazma izinlerine sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-779">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="662e9-780">Güncelleştirilmiş *Web. config* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-780">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="662e9-781">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-781">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-782">*Günlükler* klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-782">Navigate to the *logs* folder.</span></span> <span data-ttu-id="662e9-783">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-783">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="662e9-784">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-784">Study the log for errors.</span></span>

<span data-ttu-id="662e9-785">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-785">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="662e9-786">*Web. config* dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-786">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-787">`false`için **stdoutLogEnabled** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-787">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="662e9-788">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-788">Save the file.</span></span>

<span data-ttu-id="662e9-789">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="662e9-789">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-790">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-790">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-791">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-791">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="662e9-792">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-792">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-793">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-793">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="662e9-794">ASP.NET Core modülü hata ayıklama günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-794">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="662e9-795">ASP.NET Core modülü hata ayıklama günlüğünü etkinleştirmek için aşağıdaki işleyici ayarlarını uygulamanın *Web. config* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-795">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="662e9-796">Günlüğü için belirtilen yolun var olduğundan ve uygulama havuzu kimliğinin konumuna yazma izinlerine sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-796">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="662e9-797">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="662e9-797">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="662e9-798">Geliştirici özel durumu sayfasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="662e9-798">Enable the Developer Exception Page</span></span>

<span data-ttu-id="662e9-799">`ASPNETCORE_ENVIRONMENT` ortam değişkeni, uygulamayı geliştirme ortamında çalıştırmak için [Web. config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) .</span><span class="sxs-lookup"><span data-stu-id="662e9-799">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="662e9-800">Ortam, ana bilgisayar tasarımcısında `UseEnvironment` tarafından uygulama başlangıcında geçersiz kılınmadığı sürece, ortam değişkenini ayarlamak, uygulama çalıştırıldığında [Geliştirici özel durum sayfasının](xref:fundamentals/error-handling) görünmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="662e9-800">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

<span data-ttu-id="662e9-801">`ASPNETCORE_ENVIRONMENT` için ortam değişkenini ayarlamak yalnızca Internet 'e açık olmayan hazırlama ve test etme sunucularında kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-801">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="662e9-802">Sorun giderme işleminden sonra *Web. config* dosyasından ortam değişkenini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-802">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="662e9-803">*Web. config*'de ortam değişkenlerini ayarlama hakkında daha fazla bilgi Için, [Aspnetcore 'un EnvironmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="662e9-803">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="662e9-804">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="662e9-804">Obtain data from an app</span></span>

<span data-ttu-id="662e9-805">Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin.</span><span class="sxs-lookup"><span data-stu-id="662e9-805">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="662e9-806">Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="662e9-806">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="662e9-807">Yavaş veya askıda olan uygulama (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-807">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="662e9-808">*Kilitlenme dökümü* , sistem belleğinin bir anlık görüntüsüdür ve uygulama kilitlenmesinin, başlatma hatasının veya yavaş uygulamanın nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-808">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="662e9-809">Uygulama kilitleniyor veya bir özel durumla karşılaşırsa</span><span class="sxs-lookup"><span data-stu-id="662e9-809">App crashes or encounters an exception</span></span>

<span data-ttu-id="662e9-810">Windows Hata Bildirimi bir döküm edinin ve çözümleyin [(WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="662e9-810">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="662e9-811">Kilitlenme döküm dosyalarını `c:\dumps`tutmak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-811">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="662e9-812">Uygulama havuzunun klasöre yazma erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-812">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="662e9-813">[Enabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-813">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="662e9-814">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-814">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="662e9-815">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-815">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="662e9-816">Uygulamayı kilitlenmenin oluşmasına neden olan koşullar altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-816">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="662e9-817">Kilitlenme gerçekleştirildikten sonra, [Disabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-817">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="662e9-818">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-818">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="662e9-819">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-819">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="662e9-820">Uygulama kilitlenmeleri ve döküm koleksiyonu tamamlandıktan sonra, uygulamanın normal olarak sonlandırılmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-820">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="662e9-821">PowerShell betiği, WER 'i uygulama başına en fazla beş döküm toplayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="662e9-821">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-822">Kilitlenme dökümleri büyük miktarda disk alanı kaplar (her birine kadar çok gigabayt kadar).</span><span class="sxs-lookup"><span data-stu-id="662e9-822">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="662e9-823">Uygulama askıda kalıyor, başlatma sırasında başarısız oluyor veya normal şekilde çalışıyor</span><span class="sxs-lookup"><span data-stu-id="662e9-823">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="662e9-824">Bir uygulama *askıda* kaldığında (yanıt vermeyi keser ancak kilitlenmez), başlatma sırasında başarısız olur veya normal şekilde çalışır. [Kullanıcı modu döküm dosyaları:](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçmek üzere en iyi aracı seçme.</span><span class="sxs-lookup"><span data-stu-id="662e9-824">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="662e9-825">Dökümü çözümle</span><span class="sxs-lookup"><span data-stu-id="662e9-825">Analyze the dump</span></span>

<span data-ttu-id="662e9-826">Bir döküm çeşitli yaklaşımlar kullanılarak analiz edilebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-826">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="662e9-827">Daha fazla bilgi için bkz. [Kullanıcı modu döküm dosyasını çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="662e9-827">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="662e9-828">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="662e9-828">Clear package caches</span></span>

<span data-ttu-id="662e9-829">Çalışan bir uygulama, geliştirme makinesindeki .NET Core SDK yükseltmeden veya uygulama içindeki paket sürümlerini değiştirirken hemen başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-829">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="662e9-830">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-830">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="662e9-831">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="662e9-831">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="662e9-832">*Bin* ve *obj* klasörlerini silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-832">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="662e9-833">Bir komut kabuğundan [DotNet NuGet yerelleri, Tümünü Temizle](/dotnet/core/tools/dotnet-nuget-locals) ' i yürüterek paket önbelleklerini temizleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-833">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="662e9-834">Paket önbelleklerini Temizleme, [NuGet. exe](https://www.nuget.org/downloads) aracı ile de gerçekleştirilebilir ve komut `nuget locals all -clear`yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-834">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="662e9-835">*NuGet. exe* , Windows masaüstü işletim sistemiyle birlikte paketlenmiş bir yüklemedir ve [NuGet Web sitesinden](https://www.nuget.org/downloads)ayrı olarak alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-835">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="662e9-836">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-836">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="662e9-837">Uygulamayı yeniden dağıtmadan önce sunucusundaki dağıtım klasöründeki tüm dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-837">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="662e9-838">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="662e9-838">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="662e9-839">Azure belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-839">Azure documentation</span></span>

* [<span data-ttu-id="662e9-840">ASP.NET Core için Application Insights</span><span class="sxs-lookup"><span data-stu-id="662e9-840">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="662e9-841">Visual Studio 'Yu kullanarak Azure App Service Web uygulamasının sorunlarını giderme bölümünde uzaktan hata ayıklama Web Apps bölümü</span><span class="sxs-lookup"><span data-stu-id="662e9-841">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="662e9-842">Azure App Service tanılamada genel bakış</span><span class="sxs-lookup"><span data-stu-id="662e9-842">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="662e9-843">Nasıl Yapılır: Azure App Service’te Uygulamaları İzleme</span><span class="sxs-lookup"><span data-stu-id="662e9-843">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="662e9-844">Visual Studio 'Yu kullanarak Azure App Service bir Web uygulamasının sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-844">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="662e9-845">Azure Web uygulamalarınızda "502 hatalı Ağ Geçidi" ve "503 hizmeti kullanılamıyor" HTTP hatalarında sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-845">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="662e9-846">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-846">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="662e9-847">Azure 'da Web Apps için uygulama performansı SSS</span><span class="sxs-lookup"><span data-stu-id="662e9-847">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="662e9-848">Azure Web uygulaması korumalı alanı (App Service çalışma zamanı yürütme sınırlamaları)</span><span class="sxs-lookup"><span data-stu-id="662e9-848">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="662e9-849">Azure Cuma: Azure App Service tanılama ve sorun giderme deneyimi (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="662e9-849">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="662e9-850">Visual Studio belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-850">Visual Studio documentation</span></span>

* [<span data-ttu-id="662e9-851">Visual Studio 2017 ' de Azure 'da IIS 'de uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="662e9-851">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="662e9-852">Visual Studio 2017 ' de uzak IIS bilgisayarında uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="662e9-852">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="662e9-853">Visual Studio kullanarak hata ayıklamayı öğrenin</span><span class="sxs-lookup"><span data-stu-id="662e9-853">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="662e9-854">Visual Studio Code belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-854">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="662e9-855">Visual Studio Code ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="662e9-855">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="662e9-856">Bu makalede, bir uygulama Azure App Service veya IIS 'ye dağıtıldığında hataların nasıl tanılanacağı hakkında genel uygulama başlatma hataları ve yönergeleri hakkında bilgi verilmektedir:</span><span class="sxs-lookup"><span data-stu-id="662e9-856">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="662e9-857">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="662e9-857">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="662e9-858">Ortak Başlangıç HTTP durum kodu senaryolarını açıklar.</span><span class="sxs-lookup"><span data-stu-id="662e9-858">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="662e9-859">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-859">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="662e9-860">Azure App Service dağıtılan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-860">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="662e9-861">IIS üzerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-861">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="662e9-862">IIS 'ye dağıtılan veya IIS Express yerel olarak çalışan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-862">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="662e9-863">Bu kılavuz hem Windows Server hem de Windows masaüstü dağıtımları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-863">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="662e9-864">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="662e9-864">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="662e9-865">Önemli güncelleştirmeler gerçekleştirirken veya paket sürümlerini değiştirirken ne yapmanız gerektiğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="662e9-865">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="662e9-866">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="662e9-866">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="662e9-867">Ek sorun giderme konularını listeler.</span><span class="sxs-lookup"><span data-stu-id="662e9-867">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="662e9-868">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="662e9-868">App startup errors</span></span>

<span data-ttu-id="662e9-869">Visual Studio 'da bir ASP.NET Core projesi, hata ayıklama sırasında [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) barındırmak için varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="662e9-869">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="662e9-870">Yerel olarak hata ayıklamada oluşan *502,5 Işlem hatası* , bu konudaki öneri kullanılarak tanılanabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-870">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

### <a name="40314-forbidden"></a><span data-ttu-id="662e9-871">403,14 yasak</span><span class="sxs-lookup"><span data-stu-id="662e9-871">403.14 Forbidden</span></span>

<span data-ttu-id="662e9-872">Uygulama başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-872">The app fails to start.</span></span> <span data-ttu-id="662e9-873">Aşağıdaki hata günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="662e9-873">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="662e9-874">Hata genellikle barındırma sisteminde, aşağıdaki senaryolardan birini içeren bozuk bir dağıtım nedeniyle oluşur:</span><span class="sxs-lookup"><span data-stu-id="662e9-874">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="662e9-875">Uygulama, barındırma sisteminde yanlış klasöre dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-875">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="662e9-876">Dağıtım işlemi, uygulamanın tüm dosyalarını ve klasörlerini barındırma sistemindeki dağıtım klasörüne taşıyamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-876">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="662e9-877">*Web. config* dosyası dağıtımda yok veya *Web. config* dosyası içerikleri hatalı biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="662e9-877">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="662e9-878">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="662e9-878">Perform the following steps:</span></span>

1. <span data-ttu-id="662e9-879">Tüm dosya ve klasörleri barındırma sistemindeki dağıtım klasöründen silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-879">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="662e9-880">Visual Studio, PowerShell veya el ile dağıtım gibi normal dağıtım yönteminizi kullanarak, uygulamanın *Yayımlama* klasörünün içeriğini barındırma sistemine yeniden dağıtın:</span><span class="sxs-lookup"><span data-stu-id="662e9-880">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="662e9-881">*Web. config* dosyasının dağıtımda mevcut olduğunu ve içeriğinin doğru olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-881">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="662e9-882">Azure App Service barındırırken, uygulamanın `D:\home\site\wwwroot` klasörüne dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-882">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="662e9-883">Uygulama IIS tarafından barındırılıyorsa, uygulamanın **IIS yöneticisinin** **temel ayarlarında**gösterilen IIS **fiziksel yoluna** dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-883">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="662e9-884">Barındırma sistemindeki dağıtımı projenin *Yayımla* klasörünün içeriğiyle karşılaştırarak uygulamanın tüm dosya ve klasörlerinin dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-884">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="662e9-885">Yayımlanan ASP.NET Core uygulamasının düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="662e9-885">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="662e9-886">*Web. config* dosyası hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="662e9-886">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="662e9-887">500 İç Sunucu Hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-887">500 Internal Server Error</span></span>

<span data-ttu-id="662e9-888">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="662e9-888">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="662e9-889">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-889">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="662e9-890">Yanıtta içerik yok olabilir veya Yanıt, tarayıcıda *500 Iç sunucu hatası* olarak görünebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-890">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="662e9-891">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="662e9-891">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="662e9-892">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-892">From the server's perspective, that's correct.</span></span> <span data-ttu-id="662e9-893">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-893">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="662e9-894">Uygulamayı sunucuda bir komut isteminde çalıştırın veya sorunu gidermek için ASP.NET Core modülü stdout günlüğünü etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="662e9-894">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="662e9-895">502.5 işlem hatası</span><span class="sxs-lookup"><span data-stu-id="662e9-895">502.5 Process Failure</span></span>

<span data-ttu-id="662e9-896">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-896">The worker process fails.</span></span> <span data-ttu-id="662e9-897">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="662e9-897">The app doesn't start.</span></span>

<span data-ttu-id="662e9-898">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) çalışan işlemini başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="662e9-898">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="662e9-899">İşlem başlatma hatasının nedeni genellikle uygulama olay günlüğündeki girişlerden ve ASP.NET Core modülü stdout günlüğünde belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-899">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="662e9-900">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="662e9-900">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="662e9-901">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-901">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="662e9-902">*Paylaşılan çerçeve* , makinede yüklü olan ve `Microsoft.AspNetCore.App`gibi bir metapackage tarafından başvurulan derleme ( *. dll* dosyaları) kümesidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-902">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="662e9-903">Metapackage başvurusu, gerekli en düşük sürümü belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-903">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="662e9-904">Daha fazla bilgi için bkz. [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="662e9-904">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="662e9-905">Bir barındırma veya uygulamanın yanlış yapılandırılması, çalışan işleminin başarısız olmasına neden olduğunda, *502,5 Işlem hata* hatası sayfası döndürülür:</span><span class="sxs-lookup"><span data-stu-id="662e9-905">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="662e9-906">Uygulama (hata kodu: '0x800700c1') başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-906">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="662e9-907">Uygulamanın derlemesi ( *. dll*) yüklenemediğinden uygulama başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="662e9-907">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="662e9-908">W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="662e9-908">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="662e9-909">Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="662e9-909">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="662e9-910">IIS yöneticisinin **uygulama havuzlarında**uygulama havuzunu seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-910">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="662e9-911">**Eylemler** panelinde **uygulama havuzunu Düzenle** altında **Gelişmiş ayarlar** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-911">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="662e9-912">**Enable 32 bit uygulamalarını**ayarla:</span><span class="sxs-lookup"><span data-stu-id="662e9-912">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="662e9-913">32-bit (x86) bir uygulama dağıtıyorsanız, değeri `True`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-913">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="662e9-914">64 bit (x64) uygulaması dağıtıyorsanız, değeri `False`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-914">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="662e9-915">Proje dosyasındaki `<Platform>` MSBuild özelliği ile uygulamanın yayınlanan bit durumuyla ilgili bir çakışma olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-915">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="662e9-916">Bağlantı sıfırlama</span><span class="sxs-lookup"><span data-stu-id="662e9-916">Connection reset</span></span>

<span data-ttu-id="662e9-917">Üstbilgiler gönderildikten sonra bir hata oluşursa, bir hata oluştuğunda sunucunun **500 Iç sunucu hatası** gönderebilmesi için çok geç olur.</span><span class="sxs-lookup"><span data-stu-id="662e9-917">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="662e9-918">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="662e9-918">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="662e9-919">Bu tür bir hata, istemcide bir *bağlantı sıfırlama* hatası olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-919">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="662e9-920">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataların giderilmesine yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-920">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="662e9-921">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="662e9-921">Default startup limits</span></span>

<span data-ttu-id="662e9-922">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) varsayılan bir *StartupTimeLimit* 120 saniye ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-922">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="662e9-923">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-923">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="662e9-924">Modülü yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğesinin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="662e9-924">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="662e9-925">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-925">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="662e9-926">Uygulama olay günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-926">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="662e9-927">Uygulama olay günlüğüne erişmek için Azure portal **sorunları Tanıla ve çöz** dikey penceresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="662e9-927">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="662e9-928">Azure portal uygulama **Hizmetleri**' nde uygulamayı açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-928">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="662e9-929">**Tanıla ve sorunları çöz '** ü seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-929">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="662e9-930">**Tanılama araçları** başlığını seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-930">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="662e9-931">**Destek Araçları**' nın altında, **uygulama olayları** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-931">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="662e9-932">**Kaynak** sütununda *IIS AspNetCoreModule* veya *IIS Aspnetcoremodule v2* girişi tarafından belirtilen en son hatayı inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-932">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="662e9-933">**Sorunları Tanıla ve çöz** dikey penceresini kullanmanın bir alternatifi, uygulama olay günlüğü dosyasını doğrudan [kudu](https://github.com/projectkudu/kudu/wiki)kullanarak incelemektir:</span><span class="sxs-lookup"><span data-stu-id="662e9-933">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="662e9-934">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-934">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-935">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-935">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-936">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-936">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-937">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-937">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-938">**LogFiles** klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-938">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="662e9-939">*EventLog. xml* dosyasının yanındaki kurşun kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-939">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="662e9-940">Günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-940">Examine the log.</span></span> <span data-ttu-id="662e9-941">En son olayları görmek için günlüğün en altına gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-941">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="662e9-942">Uygulamayı kudu konsolunda çalıştırma</span><span class="sxs-lookup"><span data-stu-id="662e9-942">Run the app in the Kudu console</span></span>

<span data-ttu-id="662e9-943">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="662e9-943">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="662e9-944">Bu hatayı saptamak için, uygulamayı [kudu](https://github.com/projectkudu/kudu/wiki) uzaktan yürütme konsolu 'nda çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="662e9-944">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="662e9-945">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-945">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="662e9-946">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-946">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-947">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-947">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-948">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-948">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="662e9-949">32 bit (x86) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="662e9-949">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="662e9-950">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="662e9-950">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="662e9-951">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-951">Run the app:</span></span>
   * <span data-ttu-id="662e9-952">Uygulama, [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-952">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="662e9-953">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-953">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="662e9-954">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-954">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="662e9-955">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="662e9-955">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="662e9-956">*ASP.NET Core {VERSION} (x86) çalışma zamanı site uzantısının yüklenmesini gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="662e9-956">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="662e9-957">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="662e9-957">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="662e9-958">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-958">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="662e9-959">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-959">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="662e9-960">64 bit (x64) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="662e9-960">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="662e9-961">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="662e9-961">**Current release**</span></span>

* <span data-ttu-id="662e9-962">Uygulama 64 bit (x64) [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-962">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="662e9-963">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-963">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="662e9-964">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-964">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="662e9-965">Uygulamayı çalıştırın: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="662e9-965">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="662e9-966">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-966">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="662e9-967">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="662e9-967">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="662e9-968">*ASP.NET Core {VERSION} (x64) çalışma zamanı site uzantısını yüklemeyi gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="662e9-968">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="662e9-969">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="662e9-969">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="662e9-970">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="662e9-970">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="662e9-971">Uygulamadan alınan konsol çıktısı, tüm hataları gösteren kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-971">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="662e9-972">ASP.NET Core modülü stdout günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-972">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="662e9-973">ASP.NET Core Module stdout günlüğü genellikle uygulama olay günlüğünde bulunmayan yararlı hata iletilerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="662e9-973">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="662e9-974">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-974">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-975">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-975">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="662e9-976">**Sorun kategorisini seçin**altında **Web uygulaması aşağı** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-976">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="662e9-977">**Önerilen çözümler** ' de **stdout günlük yeniden yönlendirmeyi etkinleştirmek**>, **Web. config dosyasını düzenlemek için kudu konsolunu açmak**üzere düğmeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-977">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="662e9-978">Kudu **Tanılama konsolunda**, dosyaları **Wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-978">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="662e9-979">Listenin altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-979">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="662e9-980">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-980">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-981">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="662e9-981">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="662e9-982">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-982">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="662e9-983">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-983">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-984">Azure portalına dönün.</span><span class="sxs-lookup"><span data-stu-id="662e9-984">Return to the Azure portal.</span></span> <span data-ttu-id="662e9-985">**GELIŞTIRME araçları** alanında **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-985">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="662e9-986">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-986">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-987">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-987">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-988">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-988">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-989">**LogFiles** klasörünü seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-989">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="662e9-990">**Değiştirilen** sütunu inceleyin ve son değiştirilme tarihiyle stdout günlüğünü düzenlemek için kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-990">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="662e9-991">Günlük dosyası açıldığında hata görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-991">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="662e9-992">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-992">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="662e9-993">Kudu **Tanılama konsolunda**, *Web. config* dosyasını açığa çıkarmak için **Wwwroot** > yolu **sitesine** dönün.</span><span class="sxs-lookup"><span data-stu-id="662e9-993">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="662e9-994">Kalem simgesini seçerek **Web. config** dosyasını tekrar açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-994">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="662e9-995">`false`için **stdoutLogEnabled** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-995">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="662e9-996">Dosyayı kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-996">Select **Save** to save the file.</span></span>

<span data-ttu-id="662e9-997">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="662e9-997">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-998">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-998">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-999">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-999">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="662e9-1000">Yalnızca uygulama başlatma sorunlarını gidermek için stdout günlüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1000">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="662e9-1001">Başlangıçtan sonra ASP.NET Core bir uygulamada genel günlüğe kaydetme için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1001">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-1002">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-1002">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="662e9-1003">Yavaş veya askıda olan uygulama (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="662e9-1003">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="662e9-1004">Bir uygulama bir istek üzerinde yavaş bir şekilde yanıt verdiğinde veya Kilitlenmelerinde, aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="662e9-1004">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="662e9-1005">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-1005">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="662e9-1006">Azure Web uygulamasında aralıklı özel durum sorunları veya performans sorunları için döküm yakalamak üzere kilitlenme tanılayıcı site uzantısı 'nı kullanın</span><span class="sxs-lookup"><span data-stu-id="662e9-1006">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="662e9-1007">İzleme kanatları</span><span class="sxs-lookup"><span data-stu-id="662e9-1007">Monitoring blades</span></span>

<span data-ttu-id="662e9-1008">İzleme dikey pencereleri, konusunda daha önce açıklanan yöntemlere alternatif bir sorun giderme deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="662e9-1008">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="662e9-1009">Bu kanatlar 500 serisi hataları tanılamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1009">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="662e9-1010">ASP.NET Core uzantılarının yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1010">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="662e9-1011">Uzantılar yüklü değilse, bunları el ile yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="662e9-1011">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="662e9-1012">**GELIŞTIRME araçları** dikey penceresinde **Uzantılar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1012">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="662e9-1013">**ASP.NET Core uzantıları** listede görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1013">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="662e9-1014">Uzantılar yüklü değilse, **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1014">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="662e9-1015">Listeden **ASP.NET Core uzantılarını** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1015">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="662e9-1016">Yasal koşulları kabul etmek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1016">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="662e9-1017">**Uzantı Ekle** dikey penceresinde **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1017">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="662e9-1018">Bilgilendirici bir açılan ileti, uzantıların başarıyla yüklenip yüklenmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1018">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="662e9-1019">Stdout günlüğü etkinleştirilmemişse, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-1019">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="662e9-1020">Azure portal, **GELIŞTIRME araçları** alanındaki **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1020">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="662e9-1021">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1021">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="662e9-1022">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1022">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="662e9-1023">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1023">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="662e9-1024">Dosya yolu > **sitesindeki** klasörleri **açın ve listenin** altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1024">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="662e9-1025">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1025">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-1026">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="662e9-1026">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="662e9-1027">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1027">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="662e9-1028">Tanılama günlüğünü etkinleştirmek için ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="662e9-1028">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="662e9-1029">Azure portal **tanılama günlükleri** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1029">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="662e9-1030">**Uygulama günlüğü (dosya sistemi)** ve **ayrıntılı hata iletileri**için **bir anahtar seçin** .</span><span class="sxs-lookup"><span data-stu-id="662e9-1030">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="662e9-1031">Dikey pencerenin üst kısmındaki **Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1031">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="662e9-1032">Başarısız istek izlemeyi, başarısız Istek olayı arabelleğe alma (FREB) günlüğü olarak da bilinen bir şekilde eklemek için **,** **başarısız istek izleme**anahtarını seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1032">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="662e9-1033">Portalda **tanılama günlükleri** dikey penceresinde hemen listelenen **günlük akışı** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1033">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="662e9-1034">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1034">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-1035">Günlük akışı verileri içinde hatanın nedeni belirtilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1035">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="662e9-1036">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bıraktığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1036">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="662e9-1037">Başarısız istek izleme günlüklerini görüntülemek için (FREB günlükleri):</span><span class="sxs-lookup"><span data-stu-id="662e9-1037">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="662e9-1038">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1038">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="662e9-1039">Kenar çubuğunun **Destek Araçları** alanından **başarısız istek izleme günlüklerini** seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1039">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="662e9-1040">[Azure App Service konusundaki Web uygulamaları için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) ve [Azure 'Daki Web Apps Için uygulama performansı SSS](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) bölümündeki başarısız istek izlemeleri bölümüne bakın: daha fazla bilgi için nasıl yaparım? başarısız istek izlemeyi açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1040">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="662e9-1041">Daha fazla bilgi için bkz. [Azure App Service Web Apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="662e9-1041">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-1042">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1042">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-1043">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-1043">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="662e9-1044">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1044">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-1045">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-1045">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="662e9-1046">IIS 'de sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-1046">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="662e9-1047">Uygulama olay günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-1047">Application Event Log (IIS)</span></span>

<span data-ttu-id="662e9-1048">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="662e9-1048">Access the Application Event Log:</span></span>

1. <span data-ttu-id="662e9-1049">Başlat menüsünü açın, *Olay Görüntüleyicisi*araması yapın ve **Olay Görüntüleyicisi** uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1049">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="662e9-1050">**Olay Görüntüleyicisi**, **Windows günlükleri** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1050">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="662e9-1051">Uygulama olay günlüğünü açmak için **uygulama** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1051">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="662e9-1052">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1052">Search for errors associated with the failing app.</span></span> <span data-ttu-id="662e9-1053">Hataların, *kaynak* sütununda *IIS aspnetcore modülünün* veya *IIS Express aspnetcore modülünün* bir değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1053">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="662e9-1054">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="662e9-1054">Run the app at a command prompt</span></span>

<span data-ttu-id="662e9-1055">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="662e9-1055">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="662e9-1056">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="662e9-1056">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="662e9-1057">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="662e9-1057">Framework-dependent deployment</span></span>

<span data-ttu-id="662e9-1058">Uygulama, [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-1058">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="662e9-1059">Bir komut isteminde, dağıtım klasörüne gidin ve uygulamanın derlemesini *DotNet. exe*ile yürüterek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1059">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="662e9-1060">Aşağıdaki komutta, \<assembly_name >: `dotnet .\<assembly_name>.dll`için uygulama derlemesinin adını yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1060">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="662e9-1061">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1061">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="662e9-1062">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="662e9-1062">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="662e9-1063">Varsayılan konak ve gönderi kullanarak `http://localhost:5000/`bir istek yapın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1063">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="662e9-1064">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1064">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="662e9-1065">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="662e9-1065">Self-contained deployment</span></span>

<span data-ttu-id="662e9-1066">Uygulama, [kendinden bağımsız bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)ise:</span><span class="sxs-lookup"><span data-stu-id="662e9-1066">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="662e9-1067">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1067">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="662e9-1068">Aşağıdaki komutta, \<assembly_name >: `<assembly_name>.exe`için uygulama derlemesinin adını yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1068">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="662e9-1069">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1069">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="662e9-1070">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="662e9-1070">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="662e9-1071">Varsayılan konak ve gönderi kullanarak `http://localhost:5000/`bir istek yapın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1071">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="662e9-1072">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1072">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="662e9-1073">ASP.NET Core Module stdout günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-1073">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="662e9-1074">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="662e9-1074">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="662e9-1075">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1075">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="662e9-1076">*Günlükler* klasörü yoksa, klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1076">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="662e9-1077">MSBuild 'in dağıtımdaki *Günlükler* klasörünü otomatik olarak oluşturmak üzere nasıl etkinleştirileceği hakkında yönergeler için, bkz. [Dizin yapısı](xref:host-and-deploy/directory-structure) konusu.</span><span class="sxs-lookup"><span data-stu-id="662e9-1077">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="662e9-1078">*Web. config* dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1078">Edit the *web.config* file.</span></span> <span data-ttu-id="662e9-1079">**StdoutLogEnabled** öğesini `true` olarak ayarlayın ve **stdoutLogFile** yolunu *Günlükler* klasörünü işaret etmek üzere değiştirin (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="662e9-1079">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="662e9-1080">yoldaki `stdout` günlük dosyası adı önekidir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1080">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="662e9-1081">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1081">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="662e9-1082">Dosya adı ön eki olarak `stdout` kullanarak, tipik bir günlük dosyası, *stdout_20180205184032_5412. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1082">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="662e9-1083">Uygulama havuzunuzun kimliğinin *Günlükler* klasörü için yazma izinlerine sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1083">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="662e9-1084">Güncelleştirilmiş *Web. config* dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1084">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="662e9-1085">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1085">Make a request to the app.</span></span>
1. <span data-ttu-id="662e9-1086">*Günlükler* klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1086">Navigate to the *logs* folder.</span></span> <span data-ttu-id="662e9-1087">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1087">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="662e9-1088">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1088">Study the log for errors.</span></span>

<span data-ttu-id="662e9-1089">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="662e9-1089">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="662e9-1090">*Web. config* dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1090">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="662e9-1091">`false`için **stdoutLogEnabled** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1091">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="662e9-1092">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1092">Save the file.</span></span>

<span data-ttu-id="662e9-1093">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="662e9-1093">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-1094">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1094">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="662e9-1095">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="662e9-1095">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="662e9-1096">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1096">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="662e9-1097">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="662e9-1097">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="662e9-1098">Geliştirici özel durumu sayfasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="662e9-1098">Enable the Developer Exception Page</span></span>

<span data-ttu-id="662e9-1099">`ASPNETCORE_ENVIRONMENT` ortam değişkeni, uygulamayı geliştirme ortamında çalıştırmak için [Web. config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) .</span><span class="sxs-lookup"><span data-stu-id="662e9-1099">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="662e9-1100">Ortam, ana bilgisayar tasarımcısında `UseEnvironment` tarafından uygulama başlangıcında geçersiz kılınmadığı sürece, ortam değişkenini ayarlamak, uygulama çalıştırıldığında [Geliştirici özel durum sayfasının](xref:fundamentals/error-handling) görünmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1100">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

<span data-ttu-id="662e9-1101">`ASPNETCORE_ENVIRONMENT` için ortam değişkenini ayarlamak yalnızca Internet 'e açık olmayan hazırlama ve test etme sunucularında kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1101">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="662e9-1102">Sorun giderme işleminden sonra *Web. config* dosyasından ortam değişkenini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1102">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="662e9-1103">*Web. config*'de ortam değişkenlerini ayarlama hakkında daha fazla bilgi Için, [Aspnetcore 'un EnvironmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1103">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="662e9-1104">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="662e9-1104">Obtain data from an app</span></span>

<span data-ttu-id="662e9-1105">Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1105">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="662e9-1106">Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="662e9-1106">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="662e9-1107">Yavaş veya askıda olan uygulama (IIS)</span><span class="sxs-lookup"><span data-stu-id="662e9-1107">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="662e9-1108">*Kilitlenme dökümü* , sistem belleğinin bir anlık görüntüsüdür ve uygulama kilitlenmesinin, başlatma hatasının veya yavaş uygulamanın nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1108">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="662e9-1109">Uygulama kilitleniyor veya bir özel durumla karşılaşırsa</span><span class="sxs-lookup"><span data-stu-id="662e9-1109">App crashes or encounters an exception</span></span>

<span data-ttu-id="662e9-1110">Windows Hata Bildirimi bir döküm edinin ve çözümleyin [(WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="662e9-1110">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="662e9-1111">Kilitlenme döküm dosyalarını `c:\dumps`tutmak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="662e9-1111">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="662e9-1112">Uygulama havuzunun klasöre yazma erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1112">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="662e9-1113">[Enabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-1113">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="662e9-1114">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-1114">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="662e9-1115">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-1115">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="662e9-1116">Uygulamayı kilitlenmenin oluşmasına neden olan koşullar altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="662e9-1116">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="662e9-1117">Kilitlenme gerçekleştirildikten sonra, [Disabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-1117">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="662e9-1118">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-1118">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="662e9-1119">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="662e9-1119">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="662e9-1120">Uygulama kilitlenmeleri ve döküm koleksiyonu tamamlandıktan sonra, uygulamanın normal olarak sonlandırılmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1120">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="662e9-1121">PowerShell betiği, WER 'i uygulama başına en fazla beş döküm toplayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1121">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="662e9-1122">Kilitlenme dökümleri büyük miktarda disk alanı kaplar (her birine kadar çok gigabayt kadar).</span><span class="sxs-lookup"><span data-stu-id="662e9-1122">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="662e9-1123">Uygulama askıda kalıyor, başlatma sırasında başarısız oluyor veya normal şekilde çalışıyor</span><span class="sxs-lookup"><span data-stu-id="662e9-1123">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="662e9-1124">Bir uygulama *askıda* kaldığında (yanıt vermeyi keser ancak kilitlenmez), başlatma sırasında başarısız olur veya normal şekilde çalışır. [Kullanıcı modu döküm dosyaları:](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçmek üzere en iyi aracı seçme.</span><span class="sxs-lookup"><span data-stu-id="662e9-1124">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="662e9-1125">Dökümü çözümle</span><span class="sxs-lookup"><span data-stu-id="662e9-1125">Analyze the dump</span></span>

<span data-ttu-id="662e9-1126">Bir döküm çeşitli yaklaşımlar kullanılarak analiz edilebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1126">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="662e9-1127">Daha fazla bilgi için bkz. [Kullanıcı modu döküm dosyasını çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="662e9-1127">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="662e9-1128">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="662e9-1128">Clear package caches</span></span>

<span data-ttu-id="662e9-1129">Çalışan bir uygulama, geliştirme makinesindeki .NET Core SDK yükseltmeden veya uygulama içindeki paket sürümlerini değiştirirken hemen başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1129">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="662e9-1130">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1130">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="662e9-1131">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="662e9-1131">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="662e9-1132">*Bin* ve *obj* klasörlerini silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1132">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="662e9-1133">Bir komut kabuğundan [DotNet NuGet yerelleri, Tümünü Temizle](/dotnet/core/tools/dotnet-nuget-locals) ' i yürüterek paket önbelleklerini temizleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1133">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="662e9-1134">Paket önbelleklerini Temizleme, [NuGet. exe](https://www.nuget.org/downloads) aracı ile de gerçekleştirilebilir ve komut `nuget locals all -clear`yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="662e9-1134">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="662e9-1135">*NuGet. exe* , Windows masaüstü işletim sistemiyle birlikte paketlenmiş bir yüklemedir ve [NuGet Web sitesinden](https://www.nuget.org/downloads)ayrı olarak alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="662e9-1135">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="662e9-1136">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1136">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="662e9-1137">Uygulamayı yeniden dağıtmadan önce sunucusundaki dağıtım klasöründeki tüm dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="662e9-1137">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="662e9-1138">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="662e9-1138">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="662e9-1139">Azure belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-1139">Azure documentation</span></span>

* [<span data-ttu-id="662e9-1140">ASP.NET Core için Application Insights</span><span class="sxs-lookup"><span data-stu-id="662e9-1140">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="662e9-1141">Visual Studio 'Yu kullanarak Azure App Service Web uygulamasının sorunlarını giderme bölümünde uzaktan hata ayıklama Web Apps bölümü</span><span class="sxs-lookup"><span data-stu-id="662e9-1141">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="662e9-1142">Azure App Service tanılamada genel bakış</span><span class="sxs-lookup"><span data-stu-id="662e9-1142">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="662e9-1143">Nasıl Yapılır: Azure App Service’te Uygulamaları İzleme</span><span class="sxs-lookup"><span data-stu-id="662e9-1143">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="662e9-1144">Visual Studio 'Yu kullanarak Azure App Service bir Web uygulamasının sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-1144">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="662e9-1145">Azure Web uygulamalarınızda "502 hatalı Ağ Geçidi" ve "503 hizmeti kullanılamıyor" HTTP hatalarında sorun giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-1145">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="662e9-1146">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="662e9-1146">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="662e9-1147">Azure 'da Web Apps için uygulama performansı SSS</span><span class="sxs-lookup"><span data-stu-id="662e9-1147">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="662e9-1148">Azure Web uygulaması korumalı alanı (App Service çalışma zamanı yürütme sınırlamaları)</span><span class="sxs-lookup"><span data-stu-id="662e9-1148">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="662e9-1149">Azure Cuma: Azure App Service tanılama ve sorun giderme deneyimi (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="662e9-1149">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="662e9-1150">Visual Studio belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-1150">Visual Studio documentation</span></span>

* [<span data-ttu-id="662e9-1151">Visual Studio 2017 ' de Azure 'da IIS 'de uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="662e9-1151">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="662e9-1152">Visual Studio 2017 ' de uzak IIS bilgisayarında uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="662e9-1152">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="662e9-1153">Visual Studio kullanarak hata ayıklamayı öğrenin</span><span class="sxs-lookup"><span data-stu-id="662e9-1153">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="662e9-1154">Visual Studio Code belgeleri</span><span class="sxs-lookup"><span data-stu-id="662e9-1154">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="662e9-1155">Visual Studio Code ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="662e9-1155">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)

::: moniker-end
