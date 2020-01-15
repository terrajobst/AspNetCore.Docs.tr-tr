---
title: Azure App Service ve IIS 'de ASP.NET Core sorunlarını giderme
author: guardrex
description: ASP.NET Core uygulamalarının Azure App Service ve Internet Information Services (IIS) dağıtımlarıyla ilgili sorunları tanılamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/10/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 23c90c33d197d26d1c4ad758449e318e20ef3760
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75952146"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="4e5ee-103">Azure App Service ve IIS 'de ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="4e5ee-104">, [Luke Latham](https://github.com/guardrex) ve, [kotalik](https://github.com/jkotalik) tarafından</span><span class="sxs-lookup"><span data-stu-id="4e5ee-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="4e5ee-105">Bu makalede, bir uygulama Azure App Service veya IIS 'ye dağıtıldığında hataların nasıl tanılanacağı hakkında genel uygulama başlatma hataları ve yönergeleri hakkında bilgi verilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="4e5ee-106">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="4e5ee-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="4e5ee-107">Ortak Başlangıç HTTP durum kodu senaryolarını açıklar.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="4e5ee-108">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="4e5ee-109">Azure App Service dağıtılan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="4e5ee-110">IIS üzerinde sorun giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="4e5ee-111">IIS 'ye dağıtılan veya IIS Express yerel olarak çalışan uygulamalar için sorun giderme önerisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="4e5ee-112">Bu kılavuz hem Windows Server hem de Windows masaüstü dağıtımları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="4e5ee-113">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="4e5ee-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="4e5ee-114">Önemli güncelleştirmeler gerçekleştirirken veya paket sürümlerini değiştirirken ne yapmanız gerektiğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="4e5ee-115">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4e5ee-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="4e5ee-116">Ek sorun giderme konularını listeler.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="4e5ee-117">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="4e5ee-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4e5ee-118">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="4e5ee-119">*502,5-Işlem hatası* veya yerel olarak hata ayıklarken oluşan *500,30-başlatma hatası* , bu konudaki öneri kullanılarak tanılanabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4e5ee-120">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="4e5ee-121">Yerel olarak hata ayıklamada oluşan *502,5 Işlem hatası* , bu konudaki öneri kullanılarak tanılanabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="4e5ee-122">403,14 yasak</span><span class="sxs-lookup"><span data-stu-id="4e5ee-122">403.14 Forbidden</span></span>

<span data-ttu-id="4e5ee-123">Uygulama başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-123">The app fails to start.</span></span> <span data-ttu-id="4e5ee-124">Aşağıdaki hata günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="4e5ee-125">Hata genellikle barındırma sisteminde, aşağıdaki senaryolardan birini içeren bozuk bir dağıtım nedeniyle oluşur:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="4e5ee-126">Uygulama, barındırma sisteminde yanlış klasöre dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="4e5ee-127">Dağıtım işlemi, uygulamanın tüm dosyalarını ve klasörlerini barındırma sistemindeki dağıtım klasörüne taşıyamadı.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="4e5ee-128">*Web. config* dosyası dağıtımda yok veya *Web. config* dosyası içerikleri hatalı biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="4e5ee-129">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-129">Perform the following steps:</span></span>

1. <span data-ttu-id="4e5ee-130">Tüm dosya ve klasörleri barındırma sistemindeki dağıtım klasöründen silin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="4e5ee-131">Visual Studio, PowerShell veya el ile dağıtım gibi normal dağıtım yönteminizi kullanarak, uygulamanın *Yayımlama* klasörünün içeriğini barındırma sistemine yeniden dağıtın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="4e5ee-132">*Web. config* dosyasının dağıtımda mevcut olduğunu ve içeriğinin doğru olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="4e5ee-133">Azure App Service barındırırken, uygulamanın `D:\home\site\wwwroot` klasörüne dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="4e5ee-134">Uygulama IIS tarafından barındırılıyorsa, uygulamanın **IIS yöneticisinin** **temel ayarlarında**gösterilen IIS **fiziksel yoluna** dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="4e5ee-135">Barındırma sistemindeki dağıtımı projenin *Yayımla* klasörünün içeriğiyle karşılaştırarak uygulamanın tüm dosya ve klasörlerinin dağıtıldığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="4e5ee-136">Yayımlanan ASP.NET Core uygulamasının düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="4e5ee-137">*Web. config* dosyası hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="4e5ee-138">500 İç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-138">500 Internal Server Error</span></span>

<span data-ttu-id="4e5ee-139">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="4e5ee-140">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="4e5ee-141">Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="4e5ee-142">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="4e5ee-143">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="4e5ee-144">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="4e5ee-145">Uygulamayı sunucuda bir komut isteminde çalıştırın veya sorunu gidermek için ASP.NET Core modülü stdout günlüğünü etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="4e5ee-146">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="4e5ee-147">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-147">The worker process fails.</span></span> <span data-ttu-id="4e5ee-148">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-148">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-149">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) .NET Core CLR 'yi bulamıyor ve işlem içi istek işleyicisini (*aspnetcorev2_inprocess. dll*) bulamıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="4e5ee-150">Kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-150">Check that:</span></span>

* <span data-ttu-id="4e5ee-151">Uygulamayı ya da hedeflediğinden [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini veya [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="4e5ee-152">ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="4e5ee-153">500.0 giden işlem işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="4e5ee-154">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-154">The worker process fails.</span></span> <span data-ttu-id="4e5ee-155">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-155">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-156">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) işlem dışı barındırma isteği işleyicisini bulamıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="4e5ee-157">Emin *aspnetcorev2_outofprocess.dll* yanında bir alt klasöründe yoksa *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="4e5ee-158">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="4e5ee-159">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-159">The worker process fails.</span></span> <span data-ttu-id="4e5ee-160">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-160">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-161">[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) bileşenleri yüklenirken bilinmeyen bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="4e5ee-162">Aşağıdaki eylemlerden birini gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-162">Take one of the following actions:</span></span>

* <span data-ttu-id="4e5ee-163">[Microsoft desteği](https://support.microsoft.com/oas/default.aspx?prid=15832) iletişim kurun ( **Geliştirici Araçları** ve **ASP.NET Core**' i seçin).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="4e5ee-164">Stack Overflow soru sorun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="4e5ee-165">[GitHub deponuzda](https://github.com/dotnet/AspNetCore)bir sorun yapın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-165">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="4e5ee-166">500.30 işlemdeki başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="4e5ee-167">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-167">The worker process fails.</span></span> <span data-ttu-id="4e5ee-168">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-168">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-169">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) .NET Core CLR 'yi işlem içi başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="4e5ee-170">İşlem başlatma hatasının nedeni genellikle uygulama olay günlüğündeki girişlerden ve ASP.NET Core modülü stdout günlüğünde belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="4e5ee-171">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-171">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="4e5ee-172">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-172">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="4e5ee-173">500,31 ANCM yerel bağımlılıklar bulunamadı</span><span class="sxs-lookup"><span data-stu-id="4e5ee-173">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="4e5ee-174">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-174">The worker process fails.</span></span> <span data-ttu-id="4e5ee-175">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-175">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-176">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) , .NET Core çalışma zamanını işlem içinde başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-176">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="4e5ee-177">Bu başlatma hatasının en yaygın nedeni, `Microsoft.NETCore.App` veya `Microsoft.AspNetCore.App` çalışma zamanının yüklenmemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-177">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="4e5ee-178">Uygulama, hedef ASP.NET Core 3,0 ' ye dağıtılmışsa ve bu sürüm makinede yoksa, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-178">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="4e5ee-179">Örnek bir hata iletisi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-179">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="4e5ee-180">Hata iletisi, yüklü tüm .NET Core sürümlerini ve uygulama tarafından istenen sürümü listeler.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-180">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="4e5ee-181">Bu hatayı onarmak için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-181">To fix this error, either:</span></span>

* <span data-ttu-id="4e5ee-182">Uygun .NET Core sürümünü makineye yükler.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-182">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="4e5ee-183">Uygulamayı, makinede bulunan .NET Core 'un bir sürümünü hedefleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-183">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="4e5ee-184">Uygulamayı [kendi kendine kapsanan bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd)olarak yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-184">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="4e5ee-185">Geliştirme aşamasında çalışırken (`ASPNETCORE_ENVIRONMENT` ortam değişkeni `Development`olarak ayarlandığında), HTTP yanıtına belirli bir hata yazılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-185">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="4e5ee-186">İşlem başlatma hatasının nedeni uygulama olay günlüğünde de bulunur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-186">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="4e5ee-187">500,32 ANCM dll yüklenemedi</span><span class="sxs-lookup"><span data-stu-id="4e5ee-187">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="4e5ee-188">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-188">The worker process fails.</span></span> <span data-ttu-id="4e5ee-189">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-189">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-190">Bu hatanın en yaygın nedeni, uygulamanın uyumsuz bir işlemci mimarisi için yayımlanmakta olması olabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-190">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="4e5ee-191">Çalışan işlemi 32 bitlik bir uygulama olarak çalışıyorsa ve uygulama 64 bit hedef için yayımlandıysa, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-191">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="4e5ee-192">Bu hatayı onarmak için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-192">To fix this error, either:</span></span>

* <span data-ttu-id="4e5ee-193">Çalışan işlemle aynı işlemci mimarisi için uygulamayı yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-193">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="4e5ee-194">Uygulamayı [çerçeveye bağlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-executables-fde)olarak yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-194">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="4e5ee-195">500,33 ANCM Istek Işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-195">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="4e5ee-196">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-196">The worker process fails.</span></span> <span data-ttu-id="4e5ee-197">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-197">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-198">Uygulama `Microsoft.AspNetCore.App` çerçevesine başvurmadı.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-198">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="4e5ee-199">Yalnızca `Microsoft.AspNetCore.App` çerçevesini hedefleyen uygulamalar [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module)tarafından barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-199">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="4e5ee-200">Bu hatayı düzeltemedi, uygulamanın `Microsoft.AspNetCore.App` çerçevesini hedeflediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-200">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="4e5ee-201">Uygulamanın hedeflediği çerçeveyi doğrulamak için `.runtimeconfig.json` denetleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-201">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="4e5ee-202">500,34 ANCM karışık barındırma modelleri desteklenmez</span><span class="sxs-lookup"><span data-stu-id="4e5ee-202">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="4e5ee-203">Çalışan işlem, aynı işlemde hem işlem içi uygulama hem de işlem dışı bir uygulama çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-203">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="4e5ee-204">Bu hatayı onarmak için uygulamaları ayrı IIS uygulama havuzlarında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-204">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="4e5ee-205">500,35 ANCM birden çok işlem Içi uygulama aynı Işlemde</span><span class="sxs-lookup"><span data-stu-id="4e5ee-205">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="4e5ee-206">Çalışan işlemi aynı işlemde birden çok işlem içi uygulama çalıştıramıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-206">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="4e5ee-207">Bu hatayı onarmak için uygulamaları ayrı IIS uygulama havuzlarında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="4e5ee-208">500,36 ANCM Işlem dışı Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-208">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="4e5ee-209">İşlem dışı istek işleyicisi, *aspnetcorev2_outofprocess. dll*, *aspnetcorev2. dll* dosyasının yanında değildir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-209">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="4e5ee-210">Bu, [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)bozuk bir yüklemesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-210">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="4e5ee-211">Bu hatayı gidermek için [.NET Core barındırma paketi](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (IIS için) veya Visual Studio (IIS Express için) yüklemesini onarın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-211">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="4e5ee-212">500,37 ANCM başlangıç zamanı sınırı Içinde başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="4e5ee-212">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="4e5ee-213">ANCM, kısımları başlangıç süresi sınırı içinde başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-213">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="4e5ee-214">Varsayılan olarak, zaman aşımı 120 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-214">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="4e5ee-215">Aynı makinede çok sayıda uygulama başlatılırken bu hata oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-215">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="4e5ee-216">Başlangıç sırasında sunucuda CPU/bellek kullanımı artışlarını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-216">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="4e5ee-217">Birden çok uygulamanın başlatma işlemini şaşırtmayı yapmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-217">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="4e5ee-218">502.5 işlem hatası</span><span class="sxs-lookup"><span data-stu-id="4e5ee-218">502.5 Process Failure</span></span>

<span data-ttu-id="4e5ee-219">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-219">The worker process fails.</span></span> <span data-ttu-id="4e5ee-220">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-220">The app doesn't start.</span></span>

<span data-ttu-id="4e5ee-221">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) çalışan işlemini başlatmaya çalışır, ancak başlatılamıyor.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-221">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="4e5ee-222">İşlem başlatma hatasının nedeni genellikle uygulama olay günlüğündeki girişlerden ve ASP.NET Core modülü stdout günlüğünde belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-222">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="4e5ee-223">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-223">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="4e5ee-224">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-224">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="4e5ee-225">*Paylaşılan çerçeve* , makinede yüklü olan ve `Microsoft.AspNetCore.App`gibi bir metapackage tarafından başvurulan derleme ( *. dll* dosyaları) kümesidir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-225">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="4e5ee-226">Metapackage başvurusu, gerekli en düşük sürümü belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-226">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="4e5ee-227">Daha fazla bilgi için bkz. [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-227">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="4e5ee-228">*502.5 işlem hatası* hata sayfası, barındırma veya uygulama yanlış yapılandırma çalışan işlemin başarısız olmasına neden olduğunda döndürülür:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-228">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="4e5ee-229">Uygulama (hata kodu: '0x800700c1') başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-229">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="4e5ee-230">Uygulama başlatılamadı uygulamanın derleme ( *.dll*) yüklenmesi tamamlanamadı.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-230">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="4e5ee-231">W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-231">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="4e5ee-232">Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-232">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="4e5ee-233">IIS Yöneticisi'nin uygulama havuzunu seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-233">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="4e5ee-234">Seçin **Gelişmiş ayarlar** altında **uygulama havuzunu Düzenle** içinde **eylemleri** paneli.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-234">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="4e5ee-235">Ayarlama **32-Bit uygulamaları etkinleştir**:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-235">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="4e5ee-236">Bir 32-bit (x86) dağıtma, uygulama ayarlarsanız değer `True`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-236">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="4e5ee-237">Bir 64-bit (x64) dağıtma, uygulama ayarlarsanız değer `False`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-237">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="4e5ee-238">Proje dosyasındaki `<Platform>` MSBuild özelliği ile uygulamanın yayınlanan bit durumuyla ilgili bir çakışma olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-238">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="4e5ee-239">Bağlantı sıfırlama</span><span class="sxs-lookup"><span data-stu-id="4e5ee-239">Connection reset</span></span>

<span data-ttu-id="4e5ee-240">Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-240">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="4e5ee-241">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-241">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="4e5ee-242">Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-242">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="4e5ee-243">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-243">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="4e5ee-244">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="4e5ee-244">Default startup limits</span></span>

<span data-ttu-id="4e5ee-245">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) varsayılan bir *StartupTimeLimit* 120 saniye ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-245">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="4e5ee-246">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-246">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="4e5ee-247">Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-247">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="4e5ee-248">Azure App Service sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-248">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="4e5ee-249">Uygulama olay günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-249">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="4e5ee-250">Uygulama olay günlüğüne erişmek için Azure portal **sorunları Tanıla ve çöz** dikey penceresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-250">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="4e5ee-251">Azure portal uygulama **Hizmetleri**' nde uygulamayı açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-251">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="4e5ee-252">**Sorunları tanılama ve çözme**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-252">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="4e5ee-253">**Tanılama araçları** başlığını seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-253">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="4e5ee-254">**Destek Araçları**' nın altında, **uygulama olayları** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-254">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="4e5ee-255">**Kaynak** sütununda *IIS AspNetCoreModule* veya *IIS Aspnetcoremodule v2* girişi tarafından belirtilen en son hatayı inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-255">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="4e5ee-256">**Sorunları Tanıla ve çöz** dikey penceresini kullanmanın bir alternatifi, uygulama olay günlüğü dosyasını doğrudan [kudu](https://github.com/projectkudu/kudu/wiki)kullanarak incelemektir:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-256">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="4e5ee-257">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-257">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4e5ee-258">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-258">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4e5ee-259">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-259">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4e5ee-260">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-260">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4e5ee-261">**LogFiles** klasörünü açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-261">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="4e5ee-262">*EventLog. xml* dosyasının yanındaki kurşun kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-262">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="4e5ee-263">Günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-263">Examine the log.</span></span> <span data-ttu-id="4e5ee-264">En son olayları görmek için günlüğün en altına gidin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-264">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="4e5ee-265">Uygulamayı kudu konsolunda çalıştırma</span><span class="sxs-lookup"><span data-stu-id="4e5ee-265">Run the app in the Kudu console</span></span>

<span data-ttu-id="4e5ee-266">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-266">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="4e5ee-267">Bu hatayı saptamak için, uygulamayı [kudu](https://github.com/projectkudu/kudu/wiki) uzaktan yürütme konsolu 'nda çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-267">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="4e5ee-268">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-268">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4e5ee-269">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-269">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4e5ee-270">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-270">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4e5ee-271">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-271">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="4e5ee-272">32 bit (x86) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-272">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="4e5ee-273">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="4e5ee-273">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="4e5ee-274">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-274">Run the app:</span></span>
   * <span data-ttu-id="4e5ee-275">Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="4e5ee-275">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="4e5ee-276">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="4e5ee-276">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="4e5ee-277">Uygulamadan alınan ve hataları gösteren konsol çıktısı, tüm Kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="4e5ee-278">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="4e5ee-278">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="4e5ee-279">*ASP.NET Core {VERSION} (x86) çalışma zamanı site uzantısının yüklenmesini gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="4e5ee-279">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="4e5ee-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="4e5ee-281">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="4e5ee-281">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="4e5ee-282">Uygulamadan alınan ve hataları gösteren konsol çıktısı, tüm Kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-282">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="4e5ee-283">64 bit (x64) uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-283">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="4e5ee-284">**Geçerli yayın**</span><span class="sxs-lookup"><span data-stu-id="4e5ee-284">**Current release**</span></span>

* <span data-ttu-id="4e5ee-285">Uygulama 64 bit (x64) [çerçeveye bağımlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd)ise:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-285">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="4e5ee-286">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="4e5ee-286">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="4e5ee-287">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="4e5ee-287">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="4e5ee-288">Uygulamayı çalıştırın: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="4e5ee-288">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="4e5ee-289">Uygulamadan alınan ve hataları gösteren konsol çıktısı, tüm Kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-289">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="4e5ee-290">**Önizleme sürümünde çalışan çerçeveye bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="4e5ee-290">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="4e5ee-291">*ASP.NET Core {VERSION} (x64) çalışma zamanı site uzantısını yüklemeyi gerektirir.*</span><span class="sxs-lookup"><span data-stu-id="4e5ee-291">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="4e5ee-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` çalışma zamanı sürümüdür)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="4e5ee-293">Uygulamayı çalıştırın: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="4e5ee-293">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="4e5ee-294">Uygulamadan alınan ve hataları gösteren konsol çıktısı, tüm Kudu konsoluna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-294">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="4e5ee-295">ASP.NET Core modülü stdout günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-295">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="4e5ee-296">ASP.NET Core Module stdout günlüğü genellikle uygulama olay günlüğünde bulunmayan yararlı hata iletilerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-296">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="4e5ee-297">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-297">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="4e5ee-298">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-298">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="4e5ee-299">**Sorun kategorisini seçin**altında **Web uygulaması aşağı** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-299">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="4e5ee-300">**Önerilen çözümler** ' de **stdout günlük yeniden yönlendirmeyi etkinleştirmek** > , **Web. config dosyasını düzenlemek için kudu konsolunu açmak**üzere düğmeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-300">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="4e5ee-301">Kudu **Tanılama konsolunda**, dosyaları **Wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-301">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="4e5ee-302">Listenin altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-302">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="4e5ee-303">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-303">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="4e5ee-304">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-304">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="4e5ee-305">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-305">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="4e5ee-306">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-306">Make a request to the app.</span></span>
1. <span data-ttu-id="4e5ee-307">Azure portala dönün.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-307">Return to the Azure portal.</span></span> <span data-ttu-id="4e5ee-308">**GELIŞTIRME araçları** alanında **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-308">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="4e5ee-309">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-309">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4e5ee-310">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-310">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4e5ee-311">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-311">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4e5ee-312">**LogFiles** klasörünü seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-312">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="4e5ee-313">**Değiştirilen** sütunu inceleyin ve son değiştirilme tarihiyle stdout günlüğünü düzenlemek için kalem simgesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-313">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="4e5ee-314">Günlük dosyası açıldığında hata görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-314">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="4e5ee-315">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-315">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="4e5ee-316">Kudu **Tanılama konsolunda**, *Web. config* dosyasını açığa çıkarmak için **Wwwroot** > yolu **sitesine** dönün.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-316">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="4e5ee-317">Kalem simgesini seçerek **Web. config** dosyasını tekrar açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-317">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="4e5ee-318">Ayarlama **stdoutLogEnabled** için `false`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-318">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="4e5ee-319">Dosyayı kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-319">Select **Save** to save the file.</span></span>

<span data-ttu-id="4e5ee-320">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-320">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="4e5ee-321">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-321">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="4e5ee-322">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-322">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="4e5ee-323">Yalnızca uygulama başlatma sorunlarını gidermek için stdout günlüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-323">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="4e5ee-324">Başlangıçtan sonra ASP.NET Core bir uygulamada genel günlüğe kaydetme için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-324">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4e5ee-325">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-325">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="4e5ee-326">ASP.NET Core modülü hata ayıklama günlüğü (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-326">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="4e5ee-327">ASP.NET Core Module hata ayıklama günlüğü, ASP.NET Core modülünden daha ayrıntılı günlük kaydı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-327">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="4e5ee-328">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-328">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="4e5ee-329">Gelişmiş tanılama günlüğünü etkinleştirmek için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-329">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="4e5ee-330">Uygulamayı gelişmiş tanılama günlüğü için yapılandırmak üzere [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-330">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="4e5ee-331">Uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-331">Redeploy the app.</span></span>
   * <span data-ttu-id="4e5ee-332">[Gelişmiş tanılama günlüklerinde](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) gösterilen `<handlerSettings>` kudu konsolunu kullanarak canlı uygulamanın *Web. config* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-332">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="4e5ee-333">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-333">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4e5ee-334">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-334">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4e5ee-335">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-335">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="4e5ee-336">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-336">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="4e5ee-337">Dosyaları **wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-337">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="4e5ee-338">*Web. config* dosyasını, kurşun kalem düğmesini seçerek düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-338">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="4e5ee-339">`<handlerSettings>` bölümünü, [Gelişmiş tanılama günlüklerinde](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-339">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="4e5ee-340">**Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-340">Select the **Save** button.</span></span>
1. <span data-ttu-id="4e5ee-341">**Gelişmiş araçları** **geliştirme araçları** alanında açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-341">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4e5ee-342">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-342">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4e5ee-343">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-343">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4e5ee-344">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-344">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4e5ee-345">Dosyaları **wwwroot** > yol **sitesine** açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-345">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="4e5ee-346">*Aspnetcore-Debug. log* dosyası için bir yol sağlamadıysanız dosya listede görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-346">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="4e5ee-347">Bir yol sağladıysanız, günlük dosyasının konumuna gidin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-347">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="4e5ee-348">Dosya adının yanındaki kurşun kalem düğmesiyle günlük dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-348">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="4e5ee-349">Sorun giderme tamamlandığında hata ayıklama günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-349">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="4e5ee-350">Gelişmiş hata ayıklama günlüğünü devre dışı bırakmak için aşağıdakilerden birini yapın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-350">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="4e5ee-351">*Web. config* dosyasından `<handlerSettings>` yerel olarak kaldırın ve uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-351">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="4e5ee-352">*Web. config* dosyasını düzenlemek ve `<handlerSettings>` bölümünü kaldırmak Için kudu konsolunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-352">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="4e5ee-353">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-353">Save the file.</span></span>

<span data-ttu-id="4e5ee-354">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-354">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="4e5ee-355">Hata ayıklama günlüğünü devre dışı bırakma hatası, uygulama veya sunucu hatasına yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-355">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="4e5ee-356">Günlük dosyası boyutunda sınır yoktur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-356">There's no limit on log file size.</span></span> <span data-ttu-id="4e5ee-357">Yalnızca uygulama başlatma sorunlarını gidermek için hata ayıklama günlüğünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-357">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="4e5ee-358">Başlangıçtan sonra ASP.NET Core bir uygulamada genel günlüğe kaydetme için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-358">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4e5ee-359">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-359">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="4e5ee-360">Yavaş veya askıda olan uygulama (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-360">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="4e5ee-361">Bir uygulama bir istek üzerinde yavaş bir şekilde yanıt verdiğinde veya Kilitlenmelerinde, aşağıdaki makalelere bakın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-361">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="4e5ee-362">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-362">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="4e5ee-363">Azure Web uygulamasında aralıklı özel durum sorunları veya performans sorunları için döküm yakalamak üzere kilitlenme tanılayıcı site uzantısı 'nı kullanın</span><span class="sxs-lookup"><span data-stu-id="4e5ee-363">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="4e5ee-364">İzleme kanatları</span><span class="sxs-lookup"><span data-stu-id="4e5ee-364">Monitoring blades</span></span>

<span data-ttu-id="4e5ee-365">İzleme dikey pencereleri, konusunda daha önce açıklanan yöntemlere alternatif bir sorun giderme deneyimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-365">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="4e5ee-366">Bu kanatlar 500 serisi hataları tanılamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-366">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="4e5ee-367">ASP.NET Core uzantılarının yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-367">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="4e5ee-368">Uzantılar yüklü değilse, bunları el ile yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-368">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="4e5ee-369">**GELIŞTIRME araçları** dikey penceresinde **Uzantılar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-369">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="4e5ee-370">**ASP.NET Core uzantıları** listede görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-370">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="4e5ee-371">Uzantılar yüklü değilse, **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-371">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="4e5ee-372">Listeden **ASP.NET Core uzantılarını** seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-372">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="4e5ee-373">Yasal koşulları kabul etmek için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-373">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="4e5ee-374">**Uzantı Ekle** dikey penceresinde **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-374">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="4e5ee-375">Bilgilendirici bir açılan ileti, uzantıların başarıyla yüklenip yüklenmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-375">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="4e5ee-376">Stdout günlüğü etkinleştirilmemişse, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-376">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="4e5ee-377">Azure portal, **GELIŞTIRME araçları** alanındaki **Gelişmiş Araçlar** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-377">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="4e5ee-378">**Git&rarr;** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-378">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4e5ee-379">Kudu konsolu yeni bir tarayıcı sekmesi veya penceresinde açılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-379">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4e5ee-380">Sayfanın üst kısmındaki gezinti çubuğunu kullanarak **hata ayıklama konsolu 'nu** açın ve **cmd**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-380">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4e5ee-381">Dosya yolu > **sitesindeki** klasörleri **açın ve listenin** altındaki *Web. config* dosyasını açığa çıkarmak için aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-381">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="4e5ee-382">*Web. config* dosyasının yanındaki kurşun kalem simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-382">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="4e5ee-383">**StdoutLogEnabled** olarak ayarlayın ve **stdoutLogFile** yolunu `true` olarak değiştirin: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-383">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="4e5ee-384">Güncelleştirilmiş *Web. config* dosyasını kaydetmek için **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-384">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="4e5ee-385">Tanılama günlüğünü etkinleştirmek için ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-385">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="4e5ee-386">Azure portal **tanılama günlükleri** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-386">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="4e5ee-387">**Uygulama günlüğü (dosya sistemi)** ve **ayrıntılı hata iletileri**için **bir anahtar seçin** .</span><span class="sxs-lookup"><span data-stu-id="4e5ee-387">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="4e5ee-388">Dikey pencerenin üst kısmındaki **Kaydet** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-388">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="4e5ee-389">Başarısız istek izlemeyi, başarısız Istek olayı arabelleğe alma (FREB) günlüğü olarak da bilinen bir şekilde eklemek için **,** **başarısız istek izleme**anahtarını seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-389">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="4e5ee-390">Portalda **tanılama günlükleri** dikey penceresinde hemen listelenen **günlük akışı** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-390">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="4e5ee-391">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-391">Make a request to the app.</span></span>
1. <span data-ttu-id="4e5ee-392">Günlük akışı verileri içinde hatanın nedeni belirtilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-392">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="4e5ee-393">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bıraktığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-393">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="4e5ee-394">Başarısız istek izleme günlüklerini görüntülemek için (FREB günlükleri):</span><span class="sxs-lookup"><span data-stu-id="4e5ee-394">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="4e5ee-395">Azure portal **sorunları Tanıla ve çöz** dikey penceresine gidin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-395">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="4e5ee-396">Kenar çubuğunun **Destek Araçları** alanından **başarısız istek izleme günlüklerini** seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-396">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="4e5ee-397">[Azure App Service konusundaki Web uygulamaları için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) ve [Azure 'Daki Web Apps Için uygulama performansı SSS](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) bölümündeki başarısız istek izlemeleri bölümüne bakın: daha fazla bilgi için nasıl yaparım? başarısız istek izlemeyi açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-397">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="4e5ee-398">Daha fazla bilgi için bkz. [Azure App Service Web Apps için tanılama günlüğünü etkinleştirme](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-398">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="4e5ee-399">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-399">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="4e5ee-400">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-400">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="4e5ee-401">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-401">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4e5ee-402">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-402">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="4e5ee-403">IIS 'de sorun giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-403">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="4e5ee-404">Uygulama olay günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-404">Application Event Log (IIS)</span></span>

<span data-ttu-id="4e5ee-405">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-405">Access the Application Event Log:</span></span>

1. <span data-ttu-id="4e5ee-406">Başlat menüsünü açın, *Olay Görüntüleyicisi*araması yapın ve **Olay Görüntüleyicisi** uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-406">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="4e5ee-407">İçinde **Olay Görüntüleyicisi'ni**açın **Windows Günlükleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-407">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="4e5ee-408">Seçin **uygulama** uygulama olay günlüğünü açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-408">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="4e5ee-409">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-409">Search for errors associated with the failing app.</span></span> <span data-ttu-id="4e5ee-410">Hata içeren bir değeri *IIS AspNetCore Modülü* veya *IIS Express AspNetCore Modülü* içinde *kaynak* sütun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-410">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="4e5ee-411">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="4e5ee-411">Run the app at a command prompt</span></span>

<span data-ttu-id="4e5ee-412">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-412">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="4e5ee-413">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-413">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="4e5ee-414">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="4e5ee-414">Framework-dependent deployment</span></span>

<span data-ttu-id="4e5ee-415">Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="4e5ee-415">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="4e5ee-416">Bir komut isteminde, dağıtım klasörüne gidin ve uygulama ile uygulamanın derleme yürüterek çalıştırma *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-416">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="4e5ee-417">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-417">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="4e5ee-418">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-418">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="4e5ee-419">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-419">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="4e5ee-420">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-420">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="4e5ee-421">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-421">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="4e5ee-422">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="4e5ee-422">Self-contained deployment</span></span>

<span data-ttu-id="4e5ee-423">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="4e5ee-423">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="4e5ee-424">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-424">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="4e5ee-425">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-425">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="4e5ee-426">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-426">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="4e5ee-427">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-427">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="4e5ee-428">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-428">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="4e5ee-429">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-429">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="4e5ee-430">ASP.NET Core Module stdout günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-430">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="4e5ee-431">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-431">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="4e5ee-432">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-432">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="4e5ee-433">Varsa *günlükleri* klasör mevcut değilse, bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-433">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="4e5ee-434">Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak dağıtım klasörüne bakın [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-434">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="4e5ee-435">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-435">Edit the *web.config* file.</span></span> <span data-ttu-id="4e5ee-436">Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu işaret edecek şekilde *günlükleri* klasör (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-436">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="4e5ee-437">`stdout` Günlük dosyası adı ön eki içinde yoludur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-437">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="4e5ee-438">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-438">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="4e5ee-439">Kullanarak `stdout` dosya adı ön eki genel günlük dosyası adında *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-439">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="4e5ee-440">Uygulama havuzunun kimliği için yazma izinlerine sahip olduğundan emin olun *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-440">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="4e5ee-441">Güncelleştirilmiş Kaydet *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-441">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="4e5ee-442">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-442">Make a request to the app.</span></span>
1. <span data-ttu-id="4e5ee-443">Gidin *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-443">Navigate to the *logs* folder.</span></span> <span data-ttu-id="4e5ee-444">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-444">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="4e5ee-445">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-445">Study the log for errors.</span></span>

<span data-ttu-id="4e5ee-446">Sorun giderme tamamlandığında stdout günlüğünü devre dışı bırak:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-446">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="4e5ee-447">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-447">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="4e5ee-448">Ayarlama **stdoutLogEnabled** için `false`.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-448">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="4e5ee-449">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-449">Save the file.</span></span>

<span data-ttu-id="4e5ee-450">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-450">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="4e5ee-451">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-451">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="4e5ee-452">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-452">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="4e5ee-453">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-453">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4e5ee-454">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-454">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="4e5ee-455">ASP.NET Core modülü hata ayıklama günlüğü (IIS)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-455">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="4e5ee-456">ASP.NET Core modülü hata ayıklama günlüğünü etkinleştirmek için aşağıdaki işleyici ayarlarını uygulamanın *Web. config* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-456">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="4e5ee-457">Günlüğü için belirtilen yolun var olduğundan ve uygulama havuzu kimliğinin konumuna yazma izinlerine sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-457">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="4e5ee-458">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-458">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="4e5ee-459">Geliştirici özel durumu sayfasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="4e5ee-459">Enable the Developer Exception Page</span></span>

<span data-ttu-id="4e5ee-460">`ASPNETCORE_ENVIRONMENT` [Ortam değişkeni web.config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) uygulama geliştirme ortamında çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-460">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="4e5ee-461">Ortama göre uygulama başlangıç kılmadığınız sürece `UseEnvironment` konak Oluşturucusu'ortam değişkenini ayarlayarak sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesini zaman uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-461">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

::: moniker-end

<span data-ttu-id="4e5ee-462">İçin ortam değişkenini ayarlayarak `ASPNETCORE_ENVIRONMENT` yalnızca Internet'e açık olmayan sunucuları test ve hazırlık kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-462">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="4e5ee-463">Ortam değişkeninden kaldırmak *web.config* sorun giderme sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-463">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="4e5ee-464">Ortam değişkenlerini ayarlama hakkında bilgi *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-464">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="4e5ee-465">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="4e5ee-465">Obtain data from an app</span></span>

<span data-ttu-id="4e5ee-466">Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-466">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="4e5ee-467">Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-467">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="4e5ee-468">Yavaş veya askıda olan uygulama (IIS)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-468">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="4e5ee-469">*Kilitlenme dökümü* , sistem belleğinin bir anlık görüntüsüdür ve uygulama kilitlenmesinin, başlatma hatasının veya yavaş uygulamanın nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-469">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="4e5ee-470">Uygulama kilitleniyor veya bir özel durumla karşılaşırsa</span><span class="sxs-lookup"><span data-stu-id="4e5ee-470">App crashes or encounters an exception</span></span>

<span data-ttu-id="4e5ee-471">Windows Hata Bildirimi bir döküm edinin ve çözümleyin [(WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="4e5ee-471">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="4e5ee-472">Kilitlenme döküm dosyalarını `c:\dumps`tutmak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-472">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="4e5ee-473">Uygulama havuzunun klasöre yazma erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-473">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="4e5ee-474">[Enabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-474">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="4e5ee-475">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-475">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="4e5ee-476">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-476">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="4e5ee-477">Uygulamayı kilitlenmenin oluşmasına neden olan koşullar altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-477">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="4e5ee-478">Kilitlenme gerçekleştirildikten sonra, [Disabledökümler PowerShell betiğini](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1)çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-478">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="4e5ee-479">Uygulama, [işlem içi barındırma modelini](xref:host-and-deploy/iis/index#in-process-hosting-model)kullanıyorsa, *W3wp. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-479">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="4e5ee-480">Uygulama [işlem dışı barındırma modelini](xref:host-and-deploy/iis/index#out-of-process-hosting-model)kullanıyorsa, *DotNet. exe*için betiği çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-480">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="4e5ee-481">Uygulama kilitlenmeleri ve döküm koleksiyonu tamamlandıktan sonra, uygulamanın normal olarak sonlandırılmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-481">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="4e5ee-482">PowerShell betiği, WER 'i uygulama başına en fazla beş döküm toplayacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-482">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="4e5ee-483">Kilitlenme dökümleri büyük miktarda disk alanı kaplar (her birine kadar çok gigabayt kadar).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-483">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="4e5ee-484">Uygulama askıda kalıyor, başlatma sırasında başarısız oluyor veya normal şekilde çalışıyor</span><span class="sxs-lookup"><span data-stu-id="4e5ee-484">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="4e5ee-485">Bir uygulama *askıda* kaldığında (yanıt vermeyi keser ancak kilitlenmez), başlatma sırasında başarısız olur veya normal şekilde çalışır. [Kullanıcı modu döküm dosyaları:](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçmek üzere en iyi aracı seçme.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-485">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="4e5ee-486">Dökümü çözümle</span><span class="sxs-lookup"><span data-stu-id="4e5ee-486">Analyze the dump</span></span>

<span data-ttu-id="4e5ee-487">Bir döküm çeşitli yaklaşımlar kullanılarak analiz edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-487">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="4e5ee-488">Daha fazla bilgi için bkz. [Kullanıcı modu döküm dosyasını çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-488">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="4e5ee-489">Paket önbelleklerini temizle</span><span class="sxs-lookup"><span data-stu-id="4e5ee-489">Clear package caches</span></span>

<span data-ttu-id="4e5ee-490">Çalışan bir uygulama, geliştirme makinesindeki .NET Core SDK yükseltmeden veya uygulama içindeki paket sürümlerini değiştirirken hemen başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-490">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="4e5ee-491">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-491">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="4e5ee-492">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="4e5ee-492">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="4e5ee-493">Silme *bin* ve *obj* klasörleri.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-493">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="4e5ee-494">Bir komut kabuğundan [DotNet NuGet yerelleri, Tümünü Temizle](/dotnet/core/tools/dotnet-nuget-locals) ' i yürüterek paket önbelleklerini temizleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-494">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="4e5ee-495">Paket önbelleklerini Temizleme, [NuGet. exe](https://www.nuget.org/downloads) aracı ile de gerçekleştirilebilir ve komut `nuget locals all -clear`yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-495">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="4e5ee-496">*nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen bir yükleme değildir ve gelen ayrı olarak edinilmelidir [NuGet Web sitesi](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="4e5ee-496">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="4e5ee-497">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-497">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="4e5ee-498">Uygulamayı yeniden dağıtmadan önce sunucusundaki dağıtım klasöründeki tüm dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="4e5ee-498">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e5ee-499">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4e5ee-499">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="4e5ee-500">Azure belgeleri</span><span class="sxs-lookup"><span data-stu-id="4e5ee-500">Azure documentation</span></span>

* [<span data-ttu-id="4e5ee-501">ASP.NET Core için Application Insights</span><span class="sxs-lookup"><span data-stu-id="4e5ee-501">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="4e5ee-502">Visual Studio 'Yu kullanarak Azure App Service Web uygulamasının sorunlarını giderme bölümünde uzaktan hata ayıklama Web Apps bölümü</span><span class="sxs-lookup"><span data-stu-id="4e5ee-502">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="4e5ee-503">Azure App Service tanılamada genel bakış</span><span class="sxs-lookup"><span data-stu-id="4e5ee-503">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="4e5ee-504">Nasıl Yapılır: Azure App Service’te Uygulamaları İzleme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-504">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="4e5ee-505">Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-505">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="4e5ee-506">Azure Web uygulamalarınızda "502 hatalı Ağ Geçidi" ve "503 hizmeti kullanılamıyor" HTTP hatalarında sorun giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-506">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="4e5ee-507">Azure App Service web uygulamasında yavaş performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4e5ee-507">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="4e5ee-508">Azure 'da Web Apps için uygulama performansı SSS</span><span class="sxs-lookup"><span data-stu-id="4e5ee-508">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="4e5ee-509">Azure Web uygulaması korumalı alanı (App Service çalışma zamanı yürütme sınırlamaları)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-509">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="4e5ee-510">Azure Cuma: Azure App Service tanılama ve sorun giderme deneyimi (12 dakikalık video)</span><span class="sxs-lookup"><span data-stu-id="4e5ee-510">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="4e5ee-511">Visual Studio belgeleri</span><span class="sxs-lookup"><span data-stu-id="4e5ee-511">Visual Studio documentation</span></span>

* [<span data-ttu-id="4e5ee-512">Visual Studio 2017 ' de Azure 'da IIS 'de uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e5ee-512">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="4e5ee-513">Visual Studio 2017 ' de uzak IIS bilgisayarında uzaktan hata ayıklama ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e5ee-513">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="4e5ee-514">Visual Studio kullanarak hata ayıklamayı öğrenin</span><span class="sxs-lookup"><span data-stu-id="4e5ee-514">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="4e5ee-515">Visual Studio Code belgeleri</span><span class="sxs-lookup"><span data-stu-id="4e5ee-515">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="4e5ee-516">Visual Studio kodu ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="4e5ee-516">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
