---
title: IIS üzerinde ASP.NET Core sorunlarını giderme
author: guardrex
description: Internet Information Services (IIS) ASP.NET Core uygulamaları dağıtımlarına sorunları tanılamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: f89eac3ae6fc704bc8bf38a9707fc3c6c3568e91
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899411"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="dc313-103">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="dc313-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="dc313-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dc313-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dc313-105">Bu makalede yönergeler bir ASP.NET Core tanılamak uygulama başlatma çıkış ile barındırırken sağlar [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="dc313-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="dc313-106">Bu makaledeki bilgiler, IIS'de Windows Server ve Windows Masaüstü barındırma için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="dc313-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dc313-107">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="dc313-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="dc313-108">A *502.5 - işlem hatası* veya *500.30 - başlangıç hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="dc313-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dc313-109">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="dc313-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="dc313-110">A *502.5 işlem hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="dc313-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="dc313-111">Ek sorun giderme konuları:</span><span class="sxs-lookup"><span data-stu-id="dc313-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="dc313-112"><xref:host-and-deploy/azure-apps/troubleshoot> App Service kullansa [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve uygulamaları barındırın, IIS, App Service için özel yönergeler için adanmış konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="dc313-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="dc313-113"><xref:fundamentals/error-handling> Yerel bir sistemde geliştirme sırasında ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.</span><span class="sxs-lookup"><span data-stu-id="dc313-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="dc313-114">[Visual Studio kullanarak hata ayıklamayı öğrenin](/visualstudio/debugger/getting-started-with-the-debugger) Bu konu Visual Studio hata ayıklayıcısını özelliklerini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="dc313-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="dc313-115">[Visual Studio kodu ile hata ayıklama](https://code.visualstudio.com/docs/editor/debugging) Visual Studio Code ile oluşturulmuş hata ayıklama desteği hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="dc313-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="dc313-116">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="dc313-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="dc313-117">502.5 işlem hatası</span><span class="sxs-lookup"><span data-stu-id="dc313-117">502.5 Process Failure</span></span>

<span data-ttu-id="dc313-118">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-118">The worker process fails.</span></span> <span data-ttu-id="dc313-119">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="dc313-119">The app doesn't start.</span></span>

<span data-ttu-id="dc313-120">Arka uç dotnet işlemini başlatmak gereken ASP.NET Core modülü çalışır ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="dc313-121">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="dc313-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="dc313-122">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="dc313-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="dc313-123">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="dc313-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="dc313-124">*Paylaşılan çerçeve* derlemeleri kümesidir (*.dll* dosyaları) bu makinede yüklü ve gibi bir metapackage tarafından başvurulan `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="dc313-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="dc313-125">Gerekli en düşük sürüm metapackage başvuru belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc313-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="dc313-126">Daha fazla bilgi için [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="dc313-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="dc313-127">*502.5 işlem hatası* hata sayfası, barındırma veya uygulama yanlış yapılandırma çalışan işlemin başarısız olmasına neden olduğunda döndürülür:</span><span class="sxs-lookup"><span data-stu-id="dc313-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![502.5 işlem hatası sayfasını gösteren bir tarayıcı penceresi](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="dc313-129">500.30 işlemdeki başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="dc313-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="dc313-130">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-130">The worker process fails.</span></span> <span data-ttu-id="dc313-131">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="dc313-131">The app doesn't start.</span></span>

<span data-ttu-id="dc313-132">İşlemdeki .NET Core CLR başlatmak gereken ASP.NET Core modülü çalışır, ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="dc313-133">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="dc313-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="dc313-134">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="dc313-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="dc313-135">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="dc313-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="dc313-136">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="dc313-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="dc313-137">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-137">The worker process fails.</span></span> <span data-ttu-id="dc313-138">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="dc313-138">The app doesn't start.</span></span>

<span data-ttu-id="dc313-139">.NET Core CLR bulun ve işlemde istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="dc313-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="dc313-140">Kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="dc313-140">Check that:</span></span>

* <span data-ttu-id="dc313-141">Uygulamayı ya da hedeflediğinden [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini veya [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dc313-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="dc313-142">ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.</span><span class="sxs-lookup"><span data-stu-id="dc313-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="dc313-143">500.0 giden işlem işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="dc313-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="dc313-144">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-144">The worker process fails.</span></span> <span data-ttu-id="dc313-145">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="dc313-145">The app doesn't start.</span></span>

<span data-ttu-id="dc313-146">İşlem dışı barındırma istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="dc313-147">Emin *aspnetcorev2_outofprocess.dll* yanında bir alt klasöründe yoksa *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="dc313-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="dc313-148">500 İç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="dc313-148">500 Internal Server Error</span></span>

<span data-ttu-id="dc313-149">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="dc313-149">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="dc313-150">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="dc313-150">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="dc313-151">Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="dc313-151">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="dc313-152">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="dc313-152">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="dc313-153">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="dc313-153">From the server's perspective, that's correct.</span></span> <span data-ttu-id="dc313-154">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="dc313-154">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="dc313-155">[Uygulamayı bir komut isteminde aşağıdakini çalıştırın](#run-the-app-at-a-command-prompt) sunucuda veya [ASP.NET Core modülü stdout günlüğünü etkinleştir](#aspnet-core-module-stdout-log) sorunu gidermek için.</span><span class="sxs-lookup"><span data-stu-id="dc313-155">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="dc313-156">Uygulama (hata kodu: '0x800700c1') başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="dc313-156">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="dc313-157">Uygulama başlatılamadı uygulamanın derleme (*.dll*) yüklenmesi tamamlanamadı.</span><span class="sxs-lookup"><span data-stu-id="dc313-157">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="dc313-158">W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="dc313-158">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="dc313-159">Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="dc313-159">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="dc313-160">IIS Yöneticisi'nin uygulama havuzunu seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="dc313-160">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="dc313-161">Seçin **Gelişmiş ayarlar** altında **uygulama havuzunu Düzenle** içinde **eylemleri** paneli.</span><span class="sxs-lookup"><span data-stu-id="dc313-161">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="dc313-162">Ayarlama **32-Bit uygulamaları etkinleştir**:</span><span class="sxs-lookup"><span data-stu-id="dc313-162">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="dc313-163">Bir 32-bit (x86) dağıtma, uygulama ayarlarsanız değer `True`.</span><span class="sxs-lookup"><span data-stu-id="dc313-163">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="dc313-164">Bir 64-bit (x64) dağıtma, uygulama ayarlarsanız değer `False`.</span><span class="sxs-lookup"><span data-stu-id="dc313-164">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="dc313-165">Bağlantı sıfırlama</span><span class="sxs-lookup"><span data-stu-id="dc313-165">Connection reset</span></span>

<span data-ttu-id="dc313-166">Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda.</span><span class="sxs-lookup"><span data-stu-id="dc313-166">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="dc313-167">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="dc313-167">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="dc313-168">Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata.</span><span class="sxs-lookup"><span data-stu-id="dc313-168">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="dc313-169">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-169">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="dc313-170">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="dc313-170">Default startup limits</span></span>

<span data-ttu-id="dc313-171">ASP.NET Core modülü ile bir varsayılan yapılandırılmış *startupTimeLimit* 120 saniye.</span><span class="sxs-lookup"><span data-stu-id="dc313-171">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="dc313-172">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-172">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="dc313-173">Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="dc313-173">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="dc313-174">Uygulama başlatma hatalarını giderme</span><span class="sxs-lookup"><span data-stu-id="dc313-174">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="dc313-175">ASP.NET Core modülü hata ayıklama günlüğünü etkinleştir</span><span class="sxs-lookup"><span data-stu-id="dc313-175">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="dc313-176">Uygulamanın aşağıdaki işleyicisi ayarları ekleyerek *web.config* ASP.NET Core modülü hata ayıklama günlükleri için dosyası:</span><span class="sxs-lookup"><span data-stu-id="dc313-176">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="dc313-177">Günlüğü için belirtilen yolun var olduğundan ve uygulama havuzu kimliğinin konumuna yazma izinlerine sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="dc313-177">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="dc313-178">Uygulama olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="dc313-178">Application Event Log</span></span>

<span data-ttu-id="dc313-179">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="dc313-179">Access the Application Event Log:</span></span>

1. <span data-ttu-id="dc313-180">Başlat menüsünü açın, arama **Olay Görüntüleyicisi'ni**ve ardından **Olay Görüntüleyicisi'ni** uygulama.</span><span class="sxs-lookup"><span data-stu-id="dc313-180">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="dc313-181">İçinde **Olay Görüntüleyicisi'ni**açın **Windows Günlükleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="dc313-181">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="dc313-182">Seçin **uygulama** uygulama olay günlüğünü açın.</span><span class="sxs-lookup"><span data-stu-id="dc313-182">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="dc313-183">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="dc313-183">Search for errors associated with the failing app.</span></span> <span data-ttu-id="dc313-184">Hata içeren bir değeri *IIS AspNetCore Modülü* veya *IIS Express AspNetCore Modülü* içinde *kaynak* sütun.</span><span class="sxs-lookup"><span data-stu-id="dc313-184">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="dc313-185">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="dc313-185">Run the app at a command prompt</span></span>

<span data-ttu-id="dc313-186">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="dc313-186">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="dc313-187">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc313-187">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="dc313-188">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="dc313-188">Framework-dependent deployment</span></span>

<span data-ttu-id="dc313-189">Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="dc313-189">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="dc313-190">Bir komut isteminde, dağıtım klasörüne gidin ve uygulama ile uygulamanın derleme yürüterek çalıştırma *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="dc313-190">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="dc313-191">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="dc313-191">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="dc313-192">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="dc313-192">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="dc313-193">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="dc313-193">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="dc313-194">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="dc313-194">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="dc313-195">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="dc313-195">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="dc313-196">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="dc313-196">Self-contained deployment</span></span>

<span data-ttu-id="dc313-197">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="dc313-197">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="dc313-198">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dc313-198">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="dc313-199">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="dc313-199">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="dc313-200">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="dc313-200">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="dc313-201">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="dc313-201">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="dc313-202">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="dc313-202">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="dc313-203">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="dc313-203">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="dc313-204">ASP.NET Core modülü stdout günlüğü</span><span class="sxs-lookup"><span data-stu-id="dc313-204">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="dc313-205">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="dc313-205">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="dc313-206">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="dc313-206">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="dc313-207">Varsa *günlükleri* klasör mevcut değilse, bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dc313-207">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="dc313-208">Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak dağıtım klasörüne bakın [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="dc313-208">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="dc313-209">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dc313-209">Edit the *web.config* file.</span></span> <span data-ttu-id="dc313-210">Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu işaret edecek şekilde *günlükleri* klasör (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="dc313-210">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="dc313-211">`stdout` Günlük dosyası adı ön eki içinde yoludur.</span><span class="sxs-lookup"><span data-stu-id="dc313-211">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="dc313-212">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="dc313-212">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="dc313-213">Kullanarak `stdout` dosya adı ön eki genel günlük dosyası adında *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="dc313-213">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="dc313-214">Uygulama havuzunun kimliği için yazma izinlerine sahip olduğundan emin olun *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="dc313-214">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="dc313-215">Güncelleştirilmiş Kaydet *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dc313-215">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="dc313-216">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dc313-216">Make a request to the app.</span></span>
1. <span data-ttu-id="dc313-217">Gidin *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="dc313-217">Navigate to the *logs* folder.</span></span> <span data-ttu-id="dc313-218">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="dc313-218">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="dc313-219">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="dc313-219">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc313-220">Sorun giderme işlemi tamamlandıktan sonra stdout günlüğü devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="dc313-220">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="dc313-221">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dc313-221">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="dc313-222">Ayarlama **stdoutLogEnabled** için `false`.</span><span class="sxs-lookup"><span data-stu-id="dc313-222">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="dc313-223">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="dc313-223">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="dc313-224">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-224">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="dc313-225">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="dc313-225">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="dc313-226">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="dc313-226">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="dc313-227">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="dc313-227">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="dc313-228">Geliştirici özel durumu sayfasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="dc313-228">Enable the Developer Exception Page</span></span>

<span data-ttu-id="dc313-229">`ASPNETCORE_ENVIRONMENT` [Ortam değişkeni web.config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) uygulama geliştirme ortamında çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="dc313-229">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="dc313-230">Ortama göre uygulama başlangıç kılmadığınız sürece `UseEnvironment` konak Oluşturucusu'ortam değişkenini ayarlayarak sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesini zaman uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dc313-230">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="dc313-231">İçin ortam değişkenini ayarlayarak `ASPNETCORE_ENVIRONMENT` yalnızca Internet'e açık olmayan sunucuları test ve hazırlık kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-231">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="dc313-232">Ortam değişkeninden kaldırmak *web.config* sorun giderme sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="dc313-232">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="dc313-233">Ortam değişkenlerini ayarlama hakkında bilgi *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="dc313-233">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="dc313-234">Ortak başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="dc313-234">Common startup errors</span></span>

<span data-ttu-id="dc313-235">Bkz. <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="dc313-235">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="dc313-236">Uygulama başlatma önleyen yaygın sorunların çoğunu başvuru konusunda ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="dc313-236">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="dc313-237">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="dc313-237">Obtain data from an app</span></span>

<span data-ttu-id="dc313-238">Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin.</span><span class="sxs-lookup"><span data-stu-id="dc313-238">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="dc313-239">Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="dc313-239">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="dc313-240">Bir döküm oluşturma</span><span class="sxs-lookup"><span data-stu-id="dc313-240">Create a dump</span></span>

<span data-ttu-id="dc313-241">A *döküm* sistem belleğinin anlık görüntüsüdür ve uygulama kilitlenmesi, başlatma hatası ya da yavaş uygulama nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-241">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="dc313-242">Uygulama kilitlendiğinde veya özel bir durum karşılaştığında</span><span class="sxs-lookup"><span data-stu-id="dc313-242">App crashes or encounters an exception</span></span>

<span data-ttu-id="dc313-243">Almak ve bir dökümü analiz [Windows hata bildirimi (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="dc313-243">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="dc313-244">Kilitlenme döküm dosyaları tutmak için bir klasör oluşturun `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="dc313-244">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="dc313-245">Uygulama havuzu klasöre yazma erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dc313-245">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="dc313-246">Çalıştırma [EnableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="dc313-246">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="dc313-247">Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:fundamentals/servers/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="dc313-247">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="dc313-248">Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:fundamentals/servers/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="dc313-248">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="dc313-249">Kilitlenme durumu oluşmasına neden olan koşulları altında uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dc313-249">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="dc313-250">Kilitlenme oluştuktan sonra Çalıştır [DisableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="dc313-250">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="dc313-251">Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:fundamentals/servers/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="dc313-251">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="dc313-252">Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:fundamentals/servers/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="dc313-252">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="dc313-253">Bir uygulama kilitlenmelerini ve döküm toplama tamamlandıktan sonra uygulama normal şekilde sona izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="dc313-253">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="dc313-254">PowerShell Betiği, uygulama başına en fazla beş dökümleri toplanacak WER yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="dc313-254">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="dc313-255">Kilitlenme bilgi dökümleri, çok miktarda disk alanı (en fazla birkaç gigabayt her)'kurmak alabilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-255">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="dc313-256">Uygulama yanıt vermemeye başlıyor, başlatma sırasında başarısız olursa veya normal bir şekilde çalışır</span><span class="sxs-lookup"><span data-stu-id="dc313-256">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="dc313-257">Bir uygulama *askıda* (yanıt vermiyor ancak kilitlenme değil), bkz. normalde, başlatma veya çalıştırma sırasında başarısız oluyor [kullanıcı modu dökümü dosyaları: En uygun aracı seçme](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçin.</span><span class="sxs-lookup"><span data-stu-id="dc313-257">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="dc313-258">Dökümü analiz edin</span><span class="sxs-lookup"><span data-stu-id="dc313-258">Analyze the dump</span></span>

<span data-ttu-id="dc313-259">Bir döküm çeşitli yaklaşımlar kullanılarak çözümlenebilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-259">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="dc313-260">Daha fazla bilgi için [bir kullanıcı modu bilgi döküm dosyası çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="dc313-260">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="dc313-261">Uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="dc313-261">Remote debugging</span></span>

<span data-ttu-id="dc313-262">Bkz: [Visual Studio 2017'de uzak bir IIS bilgisayarda uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="dc313-262">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="dc313-263">Application Insights</span><span class="sxs-lookup"><span data-stu-id="dc313-263">Application Insights</span></span>

<span data-ttu-id="dc313-264">[Application Insights](/azure/application-insights/) tan alınan telemetri verilerini günlüğe kaydetme ve raporlama özellikleri hata dahil olmak üzere, IIS tarafından barındırılan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="dc313-264">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="dc313-265">Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra gerçekleşen hataları üzerinde rapor edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc313-265">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="dc313-266">Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="dc313-266">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="dc313-267">Ek öneriler</span><span class="sxs-lookup"><span data-stu-id="dc313-267">Additional advice</span></span>

<span data-ttu-id="dc313-268">Bazen ya da .NET Core SDK içinde uygulama geliştirme makine ya da paket sürümleri üzerinde hemen yükselttikten sonra çalışan bir uygulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="dc313-268">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="dc313-269">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="dc313-269">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="dc313-270">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="dc313-270">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="dc313-271">Silme *bin* ve *obj* klasörleri.</span><span class="sxs-lookup"><span data-stu-id="dc313-271">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="dc313-272">Temizleyin, paketin önbelleğe *% USERPROFILE %\\.nuget\\paketleri* ve *% LocalAppData %\\Nuget\\v3 önbellek*.</span><span class="sxs-lookup"><span data-stu-id="dc313-272">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="dc313-273">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="dc313-273">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="dc313-274">Önceki dağıtım sunucu üzerinde tamamen uygulama dağıtarak önce silindiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="dc313-274">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="dc313-275">Yürütmek için paket önbellekleri temizlemek için kullanışlı bir yol olan `dotnet nuget locals all --clear` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="dc313-275">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="dc313-276">Paket önbelleklerini temizleyerek da elde edilebilir kullanarak [nuget.exe](https://www.nuget.org/downloads) araç ve komut yürütme `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="dc313-276">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="dc313-277">*nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen bir yükleme değildir ve gelen ayrı olarak edinilmelidir [NuGet Web sitesi](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="dc313-277">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc313-278">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dc313-278">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
