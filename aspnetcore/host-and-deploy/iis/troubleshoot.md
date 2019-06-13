---
title: IIS üzerinde ASP.NET Core sorunlarını giderme
author: guardrex
description: Internet Information Services (IIS) ASP.NET Core uygulamaları dağıtımlarına sorunları tanılamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: cb42a262c89c27fa350e936184f8ddb3a02788f0
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034749"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="c7e59-103">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="c7e59-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="c7e59-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c7e59-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c7e59-105">Bu makalede yönergeler bir ASP.NET Core tanılamak uygulama başlatma çıkış ile barındırırken sağlar [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="c7e59-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="c7e59-106">Bu makaledeki bilgiler, IIS'de Windows Server ve Windows Masaüstü barındırma için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c7e59-107">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="c7e59-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="c7e59-108">A *502.5 - işlem hatası* veya *500.30 - başlangıç hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c7e59-109">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="c7e59-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="c7e59-110">A *502.5 işlem hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="c7e59-111">Ek sorun giderme konuları:</span><span class="sxs-lookup"><span data-stu-id="c7e59-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="c7e59-112"><xref:host-and-deploy/azure-apps/troubleshoot> App Service kullansa [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve uygulamaları barındırın, IIS, App Service için özel yönergeler için adanmış konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="c7e59-113"><xref:fundamentals/error-handling> Yerel bir sistemde geliştirme sırasında ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="c7e59-114">[Visual Studio kullanarak hata ayıklamayı öğrenin](/visualstudio/debugger/getting-started-with-the-debugger) Bu konu Visual Studio hata ayıklayıcısını özelliklerini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="c7e59-115">[Visual Studio kodu ile hata ayıklama](https://code.visualstudio.com/docs/editor/debugging) Visual Studio Code ile oluşturulmuş hata ayıklama desteği hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="c7e59-116">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="c7e59-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="c7e59-117">502.5 işlem hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-117">502.5 Process Failure</span></span>

<span data-ttu-id="c7e59-118">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-118">The worker process fails.</span></span> <span data-ttu-id="c7e59-119">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-119">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-120">Arka uç dotnet işlemini başlatmak gereken ASP.NET Core modülü çalışır ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="c7e59-121">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="c7e59-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="c7e59-122">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="c7e59-123">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="c7e59-124">*Paylaşılan çerçeve* derlemeleri kümesidir ( *.dll* dosyaları) bu makinede yüklü ve gibi bir metapackage tarafından başvurulan `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="c7e59-125">Gerekli en düşük sürüm metapackage başvuru belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="c7e59-126">Daha fazla bilgi için [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="c7e59-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="c7e59-127">*502.5 işlem hatası* hata sayfası, barındırma veya uygulama yanlış yapılandırma çalışan işlemin başarısız olmasına neden olduğunda döndürülür:</span><span class="sxs-lookup"><span data-stu-id="c7e59-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![502.5 işlem hatası sayfasını gösteren bir tarayıcı penceresi](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="c7e59-129">500.30 işlemdeki başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="c7e59-130">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-130">The worker process fails.</span></span> <span data-ttu-id="c7e59-131">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-131">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-132">İşlemdeki .NET Core CLR başlatmak gereken ASP.NET Core modülü çalışır, ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="c7e59-133">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="c7e59-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="c7e59-134">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="c7e59-135">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="c7e59-136">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="c7e59-137">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-137">The worker process fails.</span></span> <span data-ttu-id="c7e59-138">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-138">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-139">.NET Core CLR bulun ve işlemde istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="c7e59-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="c7e59-140">Kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="c7e59-140">Check that:</span></span>

* <span data-ttu-id="c7e59-141">Uygulamayı ya da hedeflediğinden [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini veya [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c7e59-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="c7e59-142">ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.</span><span class="sxs-lookup"><span data-stu-id="c7e59-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="c7e59-143">500.0 giden işlem işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="c7e59-144">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-144">The worker process fails.</span></span> <span data-ttu-id="c7e59-145">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-145">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-146">İşlem dışı barındırma istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="c7e59-147">Emin *aspnetcorev2_outofprocess.dll* yanında bir alt klasöründe yoksa *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="c7e59-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="c7e59-148">500.31 yerel bağımlılıkları bulmak ANCM başarısız oldu</span><span class="sxs-lookup"><span data-stu-id="c7e59-148">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="c7e59-149">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-149">The worker process fails.</span></span> <span data-ttu-id="c7e59-150">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-150">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-151">.NET Core çalışma zamanı işlemdeki başlatmak gereken ASP.NET Core modülü çalışır, ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-151">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="c7e59-152">En yaygın nedeni bu başlatma başarısız olduğunda `Microsoft.NETCore.App` veya `Microsoft.AspNetCore.App` çalışma zamanı yüklü değil.</span><span class="sxs-lookup"><span data-stu-id="c7e59-152">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="c7e59-153">ASP.NET Core 3.0 hedefine uygulamanın dağıtıldığı ve bu sürüm makinede mevcut değil, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-153">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="c7e59-154">Bir örnek hata iletisi aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="c7e59-154">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="c7e59-155">Hata iletisi, tüm yüklü .NET Core sürümleri ve uygulama tarafından istenen sürümü listelenir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-155">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="c7e59-156">Ya da bu hatayı düzeltmek için:</span><span class="sxs-lookup"><span data-stu-id="c7e59-156">To fix this error, either:</span></span>

* <span data-ttu-id="c7e59-157">Makine üzerinde .NET Core uygun sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-157">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="c7e59-158">Uygulama makinede mevcut olan .NET Core sürümünü hedefleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-158">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="c7e59-159">Uygulama olarak Yayımla bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="c7e59-159">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="c7e59-160">Geliştirme çalıştırırken ( `ASPNETCORE_ENVIRONMENT` ortam değişkeni ayarlandığında `Development`), HTTP yanıtı belirli bir hata yazılır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-160">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="c7e59-161">Bir işlem başlatma hatanın nedenini ayrıca şurada bulunur [uygulama olay günlüğüne](#application-event-log).</span><span class="sxs-lookup"><span data-stu-id="c7e59-161">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="c7e59-162">500.32 yük dll ANCM başarısız oldu</span><span class="sxs-lookup"><span data-stu-id="c7e59-162">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="c7e59-163">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-163">The worker process fails.</span></span> <span data-ttu-id="c7e59-164">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-164">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-165">Bu hatanın en yaygın nedeni, uygulama için bir uyumsuz işlemciye mimari yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-165">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="c7e59-166">Çalışan işlemin bir 32 bit uygulama olarak çalıştığı ve uygulama 64-bit hedefine yayımlandı, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-166">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="c7e59-167">Ya da bu hatayı düzeltmek için:</span><span class="sxs-lookup"><span data-stu-id="c7e59-167">To fix this error, either:</span></span>

* <span data-ttu-id="c7e59-168">Çalışan işlemi olarak aynı işlemci mimarisi için uygulamayı yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-168">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="c7e59-169">Uygulama olarak Yayımla bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="c7e59-169">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="c7e59-170">500.33 ANCM istek işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-170">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="c7e59-171">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-171">The worker process fails.</span></span> <span data-ttu-id="c7e59-172">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-172">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-173">Uygulamaya başvurmak yaramadı `Microsoft.AspNetCore.App` framework.</span><span class="sxs-lookup"><span data-stu-id="c7e59-173">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="c7e59-174">Yalnızca hedefleyen uygulamaları `Microsoft.AspNetCore.App` Framework'te ASP.NET Core modülü tarafından barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-174">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="c7e59-175">Bu hatayı düzeltmek için uygulamayı hedeflediği onaylayın `Microsoft.AspNetCore.App` framework.</span><span class="sxs-lookup"><span data-stu-id="c7e59-175">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="c7e59-176">Denetleme `.runtimeconfig.json` uygulama tarafından hedeflenen framework doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-176">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="c7e59-177">500.34 desteklenmeyen barındırma modelleri ANCM karma</span><span class="sxs-lookup"><span data-stu-id="c7e59-177">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="c7e59-178">Çalışan işlemi, hem bir işlem içi uygulamayı hem de işlem dışı uygulama aynı işlem içinde çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-178">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="c7e59-179">Bu hatayı düzeltmek için ayrı IIS uygulama havuzları, uygulamaları çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="c7e59-179">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="c7e59-180">500.35 aynı işlemde birden çok işlem içi uygulama ANCM</span><span class="sxs-lookup"><span data-stu-id="c7e59-180">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="c7e59-181">Çalışan işlemi, hem bir işlem içi uygulamayı hem de işlem dışı uygulama aynı işlem içinde çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-181">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="c7e59-182">Bu hatayı düzeltmek için ayrı IIS uygulama havuzları, uygulamaları çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="c7e59-182">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="c7e59-183">500.36 ANCM giden işlem işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-183">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="c7e59-184">İşlem dışı istek işleyicisi *aspnetcorev2_outofprocess.dll*, yanındaki değil *aspnetcorev2.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="c7e59-184">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="c7e59-185">Bu, bozuk bir ASP.NET Core modülü yüklemesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-185">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="c7e59-186">Bu hatayı düzeltmek için yüklemesini onarmak [.NET Core barındırma paket](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (IIS için) ya da Visual Studio (için IIS Express).</span><span class="sxs-lookup"><span data-stu-id="c7e59-186">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="c7e59-187">500.37 ANCM başlangıç süre sınırı içinde başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="c7e59-187">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="c7e59-188">ANCM bulunduğunda başlangıç süre sınırı içinde başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="c7e59-188">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="c7e59-189">Varsayılan olarak, zaman aşımı süresi, 120 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-189">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="c7e59-190">Çok sayıda uygulamalar aynı makinede başlatırken bu hata oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-190">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="c7e59-191">Sunucuda CPU/bellek ani başlatma sırasında kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-191">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="c7e59-192">Birden fazla uygulama başlatma işlemi basamaklandırmak gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-192">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="c7e59-193">500.30 işlemdeki başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-193">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="c7e59-194">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-194">The worker process fails.</span></span> <span data-ttu-id="c7e59-195">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-195">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-196">.NET Core çalışma zamanı işlemdeki başlatmak gereken ASP.NET Core modülü çalışır, ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-196">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="c7e59-197">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="c7e59-197">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="c7e59-198">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-198">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="c7e59-199">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-199">The worker process fails.</span></span> <span data-ttu-id="c7e59-200">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-200">The app doesn't start.</span></span>

<span data-ttu-id="c7e59-201">ASP.NET Core modülü Bileşenler yüklenirken bilinmeyen bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="c7e59-201">An unknown error occurred loading ASP.NET Core Module components.</span></span> <span data-ttu-id="c7e59-202">Aşağıdaki eylemlerden birini gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="c7e59-202">Take one of the following actions:</span></span>

* <span data-ttu-id="c7e59-203">İlgili kişi [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (seçin **Geliştirici Araçları** ardından **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="c7e59-203">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="c7e59-204">Stack Overflow sitesinde bir soru sorun.</span><span class="sxs-lookup"><span data-stu-id="c7e59-204">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="c7e59-205">Bir sorun üzerinde dosya bizim [GitHub deposu](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="c7e59-205">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="c7e59-206">500 İç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="c7e59-206">500 Internal Server Error</span></span>

<span data-ttu-id="c7e59-207">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="c7e59-207">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="c7e59-208">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-208">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="c7e59-209">Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="c7e59-209">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="c7e59-210">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-210">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="c7e59-211">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-211">From the server's perspective, that's correct.</span></span> <span data-ttu-id="c7e59-212">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="c7e59-212">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="c7e59-213">[Uygulamayı bir komut isteminde aşağıdakini çalıştırın](#run-the-app-at-a-command-prompt) sunucuda veya [ASP.NET Core modülü stdout günlüğünü etkinleştir](#aspnet-core-module-stdout-log) sorunu gidermek için.</span><span class="sxs-lookup"><span data-stu-id="c7e59-213">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="c7e59-214">Uygulama (hata kodu: '0x800700c1') başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="c7e59-214">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="c7e59-215">Uygulama başlatılamadı uygulamanın derleme ( *.dll*) yüklenmesi tamamlanamadı.</span><span class="sxs-lookup"><span data-stu-id="c7e59-215">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="c7e59-216">W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-216">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="c7e59-217">Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="c7e59-217">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="c7e59-218">IIS Yöneticisi'nin uygulama havuzunu seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="c7e59-218">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="c7e59-219">Seçin **Gelişmiş ayarlar** altında **uygulama havuzunu Düzenle** içinde **eylemleri** paneli.</span><span class="sxs-lookup"><span data-stu-id="c7e59-219">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="c7e59-220">Ayarlama **32-Bit uygulamaları etkinleştir**:</span><span class="sxs-lookup"><span data-stu-id="c7e59-220">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="c7e59-221">Bir 32-bit (x86) dağıtma, uygulama ayarlarsanız değer `True`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-221">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="c7e59-222">Bir 64-bit (x64) dağıtma, uygulama ayarlarsanız değer `False`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-222">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="c7e59-223">Bağlantı sıfırlama</span><span class="sxs-lookup"><span data-stu-id="c7e59-223">Connection reset</span></span>

<span data-ttu-id="c7e59-224">Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda.</span><span class="sxs-lookup"><span data-stu-id="c7e59-224">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="c7e59-225">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-225">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="c7e59-226">Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata.</span><span class="sxs-lookup"><span data-stu-id="c7e59-226">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="c7e59-227">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-227">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="c7e59-228">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="c7e59-228">Default startup limits</span></span>

<span data-ttu-id="c7e59-229">ASP.NET Core modülü ile bir varsayılan yapılandırılmış *startupTimeLimit* 120 saniye.</span><span class="sxs-lookup"><span data-stu-id="c7e59-229">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="c7e59-230">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-230">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="c7e59-231">Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="c7e59-231">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="c7e59-232">Uygulama başlatma hatalarını giderme</span><span class="sxs-lookup"><span data-stu-id="c7e59-232">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="c7e59-233">ASP.NET Core modülü hata ayıklama günlüğünü etkinleştir</span><span class="sxs-lookup"><span data-stu-id="c7e59-233">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="c7e59-234">Uygulamanın aşağıdaki işleyicisi ayarları ekleyerek *web.config* ASP.NET Core modülü hata ayıklama günlükleri için dosyası:</span><span class="sxs-lookup"><span data-stu-id="c7e59-234">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="c7e59-235">Günlüğü için belirtilen yolun var olduğundan ve uygulama havuzu kimliğinin konumuna yazma izinlerine sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-235">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="c7e59-236">Uygulama olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="c7e59-236">Application Event Log</span></span>

<span data-ttu-id="c7e59-237">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="c7e59-237">Access the Application Event Log:</span></span>

1. <span data-ttu-id="c7e59-238">Başlat menüsünü açın, arama **Olay Görüntüleyicisi'ni**ve ardından **Olay Görüntüleyicisi'ni** uygulama.</span><span class="sxs-lookup"><span data-stu-id="c7e59-238">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="c7e59-239">İçinde **Olay Görüntüleyicisi'ni**açın **Windows Günlükleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="c7e59-239">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="c7e59-240">Seçin **uygulama** uygulama olay günlüğünü açın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-240">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="c7e59-241">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-241">Search for errors associated with the failing app.</span></span> <span data-ttu-id="c7e59-242">Hata içeren bir değeri *IIS AspNetCore Modülü* veya *IIS Express AspNetCore Modülü* içinde *kaynak* sütun.</span><span class="sxs-lookup"><span data-stu-id="c7e59-242">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="c7e59-243">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="c7e59-243">Run the app at a command prompt</span></span>

<span data-ttu-id="c7e59-244">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="c7e59-244">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="c7e59-245">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-245">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="c7e59-246">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="c7e59-246">Framework-dependent deployment</span></span>

<span data-ttu-id="c7e59-247">Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="c7e59-247">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="c7e59-248">Bir komut isteminde, dağıtım klasörüne gidin ve uygulama ile uygulamanın derleme yürüterek çalıştırma *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="c7e59-248">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="c7e59-249">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-249">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="c7e59-250">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-250">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="c7e59-251">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="c7e59-251">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="c7e59-252">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-252">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="c7e59-253">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-253">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="c7e59-254">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="c7e59-254">Self-contained deployment</span></span>

<span data-ttu-id="c7e59-255">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="c7e59-255">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="c7e59-256">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-256">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="c7e59-257">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-257">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="c7e59-258">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-258">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="c7e59-259">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="c7e59-259">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="c7e59-260">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-260">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="c7e59-261">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-261">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="c7e59-262">ASP.NET Core modülü stdout günlüğü</span><span class="sxs-lookup"><span data-stu-id="c7e59-262">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="c7e59-263">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="c7e59-263">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="c7e59-264">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-264">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="c7e59-265">Varsa *günlükleri* klasör mevcut değilse, bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c7e59-265">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="c7e59-266">Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak dağıtım klasörüne bakın [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="c7e59-266">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="c7e59-267">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="c7e59-267">Edit the *web.config* file.</span></span> <span data-ttu-id="c7e59-268">Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu işaret edecek şekilde *günlükleri* klasör (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="c7e59-268">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="c7e59-269">`stdout` Günlük dosyası adı ön eki içinde yoludur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-269">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="c7e59-270">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-270">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="c7e59-271">Kullanarak `stdout` dosya adı ön eki genel günlük dosyası adında *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="c7e59-271">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="c7e59-272">Uygulama havuzunun kimliği için yazma izinlerine sahip olduğundan emin olun *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="c7e59-272">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="c7e59-273">Güncelleştirilmiş Kaydet *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="c7e59-273">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="c7e59-274">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c7e59-274">Make a request to the app.</span></span>
1. <span data-ttu-id="c7e59-275">Gidin *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="c7e59-275">Navigate to the *logs* folder.</span></span> <span data-ttu-id="c7e59-276">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-276">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="c7e59-277">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-277">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7e59-278">Sorun giderme işlemi tamamlandıktan sonra stdout günlüğü devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-278">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="c7e59-279">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="c7e59-279">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="c7e59-280">Ayarlama **stdoutLogEnabled** için `false`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-280">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="c7e59-281">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-281">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="c7e59-282">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-282">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="c7e59-283">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-283">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="c7e59-284">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-284">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c7e59-285">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="c7e59-285">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="c7e59-286">Geliştirici özel durumu sayfasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="c7e59-286">Enable the Developer Exception Page</span></span>

<span data-ttu-id="c7e59-287">`ASPNETCORE_ENVIRONMENT` [Ortam değişkeni web.config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) uygulama geliştirme ortamında çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="c7e59-287">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="c7e59-288">Ortama göre uygulama başlangıç kılmadığınız sürece `UseEnvironment` konak Oluşturucusu'ortam değişkenini ayarlayarak sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesini zaman uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-288">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="c7e59-289">İçin ortam değişkenini ayarlayarak `ASPNETCORE_ENVIRONMENT` yalnızca Internet'e açık olmayan sunucuları test ve hazırlık kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-289">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="c7e59-290">Ortam değişkeninden kaldırmak *web.config* sorun giderme sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="c7e59-290">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="c7e59-291">Ortam değişkenlerini ayarlama hakkında bilgi *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="c7e59-291">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="c7e59-292">Ortak başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="c7e59-292">Common startup errors</span></span>

<span data-ttu-id="c7e59-293">Bkz. <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="c7e59-293">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="c7e59-294">Uygulama başlatma önleyen yaygın sorunların çoğunu başvuru konusunda ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-294">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="c7e59-295">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="c7e59-295">Obtain data from an app</span></span>

<span data-ttu-id="c7e59-296">Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-296">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="c7e59-297">Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="c7e59-297">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="c7e59-298">Bir döküm oluşturma</span><span class="sxs-lookup"><span data-stu-id="c7e59-298">Create a dump</span></span>

<span data-ttu-id="c7e59-299">A *döküm* sistem belleğinin anlık görüntüsüdür ve uygulama kilitlenmesi, başlatma hatası ya da yavaş uygulama nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-299">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="c7e59-300">Uygulama kilitlendiğinde veya özel bir durum karşılaştığında</span><span class="sxs-lookup"><span data-stu-id="c7e59-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="c7e59-301">Almak ve bir dökümü analiz [Windows hata bildirimi (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="c7e59-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="c7e59-302">Kilitlenme döküm dosyaları tutmak için bir klasör oluşturun `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="c7e59-303">Uygulama havuzu klasöre yazma erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-303">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="c7e59-304">Çalıştırma [EnableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="c7e59-304">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="c7e59-305">Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:host-and-deploy/iis/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="c7e59-305">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="c7e59-306">Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:host-and-deploy/iis/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="c7e59-306">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="c7e59-307">Kilitlenme durumu oluşmasına neden olan koşulları altında uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-307">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="c7e59-308">Kilitlenme oluştuktan sonra Çalıştır [DisableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="c7e59-308">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="c7e59-309">Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:host-and-deploy/iis/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="c7e59-309">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="c7e59-310">Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:host-and-deploy/iis/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="c7e59-310">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="c7e59-311">Bir uygulama kilitlenmelerini ve döküm toplama tamamlandıktan sonra uygulama normal şekilde sona izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="c7e59-311">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="c7e59-312">PowerShell Betiği, uygulama başına en fazla beş dökümleri toplanacak WER yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c7e59-312">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="c7e59-313">Kilitlenme bilgi dökümleri, çok miktarda disk alanı (en fazla birkaç gigabayt her)'kurmak alabilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-313">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="c7e59-314">Uygulama yanıt vermemeye başlıyor, başlatma sırasında başarısız olursa veya normal bir şekilde çalışır</span><span class="sxs-lookup"><span data-stu-id="c7e59-314">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="c7e59-315">Bir uygulama *askıda* (yanıt vermiyor ancak kilitlenme değil), bkz. normalde, başlatma veya çalıştırma sırasında başarısız oluyor [kullanıcı modu dökümü dosyaları: En uygun aracı seçme](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-315">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="c7e59-316">Dökümü analiz edin</span><span class="sxs-lookup"><span data-stu-id="c7e59-316">Analyze the dump</span></span>

<span data-ttu-id="c7e59-317">Bir döküm çeşitli yaklaşımlar kullanılarak çözümlenebilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-317">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="c7e59-318">Daha fazla bilgi için [bir kullanıcı modu bilgi döküm dosyası çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="c7e59-318">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="c7e59-319">Uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="c7e59-319">Remote debugging</span></span>

<span data-ttu-id="c7e59-320">Bkz: [Visual Studio 2017'de uzak bir IIS bilgisayarda uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="c7e59-320">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="c7e59-321">Application Insights</span><span class="sxs-lookup"><span data-stu-id="c7e59-321">Application Insights</span></span>

<span data-ttu-id="c7e59-322">[Application Insights](/azure/application-insights/) tan alınan telemetri verilerini günlüğe kaydetme ve raporlama özellikleri hata dahil olmak üzere, IIS tarafından barındırılan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="c7e59-322">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="c7e59-323">Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra gerçekleşen hataları üzerinde rapor edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c7e59-323">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="c7e59-324">Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="c7e59-324">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="c7e59-325">Ek öneriler</span><span class="sxs-lookup"><span data-stu-id="c7e59-325">Additional advice</span></span>

<span data-ttu-id="c7e59-326">Bazen ya da .NET Core SDK içinde uygulama geliştirme makine ya da paket sürümleri üzerinde hemen yükselttikten sonra çalışan bir uygulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c7e59-326">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="c7e59-327">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="c7e59-327">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="c7e59-328">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="c7e59-328">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="c7e59-329">Silme *bin* ve *obj* klasörleri.</span><span class="sxs-lookup"><span data-stu-id="c7e59-329">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="c7e59-330">Temizleyin, paketin önbelleğe *% USERPROFILE %\\.nuget\\paketleri* ve *% LocalAppData %\\Nuget\\v3 önbellek*.</span><span class="sxs-lookup"><span data-stu-id="c7e59-330">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="c7e59-331">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="c7e59-331">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="c7e59-332">Önceki dağıtım sunucu üzerinde tamamen uygulama dağıtarak önce silindiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c7e59-332">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="c7e59-333">Yürütmek için paket önbellekleri temizlemek için kullanışlı bir yol olan `dotnet nuget locals all --clear` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="c7e59-333">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="c7e59-334">Paket önbelleklerini temizleyerek da elde edilebilir kullanarak [nuget.exe](https://www.nuget.org/downloads) araç ve komut yürütme `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="c7e59-334">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="c7e59-335">*nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen bir yükleme değildir ve gelen ayrı olarak edinilmelidir [NuGet Web sitesi](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="c7e59-335">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7e59-336">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c7e59-336">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
