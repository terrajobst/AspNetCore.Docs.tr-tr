---
title: IIS üzerinde ASP.NET Core sorunlarını giderme
author: guardrex
description: Internet Information Services (IIS) ASP.NET Core uygulamaları dağıtımlarına sorunları tanılamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2b23bf8230f7a1c207ef7870da098ffb0c597fd5
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225453"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="971cd-103">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="971cd-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="971cd-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="971cd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="971cd-105">Bu makalede yönergeler bir ASP.NET Core tanılamak uygulama başlatma çıkış ile barındırırken sağlar [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="971cd-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="971cd-106">Bu makaledeki bilgiler, IIS'de Windows Server ve Windows Masaüstü barındırma için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="971cd-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="971cd-107">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="971cd-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="971cd-108">A *502.5 - işlem hatası* veya *500.30 - başlangıç hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="971cd-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="971cd-109">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="971cd-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="971cd-110">A *502.5 işlem hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="971cd-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="971cd-111">Ek sorun giderme konuları:</span><span class="sxs-lookup"><span data-stu-id="971cd-111">Additional troubleshooting topics:</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="971cd-112">App Service kullansa [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ve uygulamaları barındırın, IIS, App Service için özel yönergeler için adanmış konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="971cd-112">Although App Service uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="971cd-113">Yerel bir sistemde geliştirme sırasında ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.</span><span class="sxs-lookup"><span data-stu-id="971cd-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="971cd-114">Visual Studio kullanarak hata ayıklamayı öğrenin</span><span class="sxs-lookup"><span data-stu-id="971cd-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="971cd-115">Bu konuda, Visual Studio hata ayıklayıcısını özelliklerini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="971cd-115">This topic introduces the features of the Visual Studio debugger.</span></span>

[<span data-ttu-id="971cd-116">Visual Studio kodu ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="971cd-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="971cd-117">Visual Studio Code ile oluşturulmuş hata ayıklama desteği hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="971cd-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="971cd-118">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="971cd-118">App startup errors</span></span>

<span data-ttu-id="971cd-119">**502.5 işlem hatası**</span><span class="sxs-lookup"><span data-stu-id="971cd-119">**502.5 Process Failure**</span></span>  
<span data-ttu-id="971cd-120">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-120">The worker process fails.</span></span> <span data-ttu-id="971cd-121">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="971cd-121">The app doesn't start.</span></span>

<span data-ttu-id="971cd-122">Arka uç dotnet işlemini başlatmak gereken ASP.NET Core modülü çalışır ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-122">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="971cd-123">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="971cd-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="971cd-124">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="971cd-124">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="971cd-125">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="971cd-125">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="971cd-126">*502.5 işlem hatası* hata sayfası, barındırma veya uygulama yanlış yapılandırma çalışan işlemin başarısız olmasına neden olduğunda döndürülür:</span><span class="sxs-lookup"><span data-stu-id="971cd-126">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![502.5 işlem hatası sayfasını gösteren bir tarayıcı penceresi](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="971cd-128">**500.30 işlemdeki başlatma hatası**</span><span class="sxs-lookup"><span data-stu-id="971cd-128">**500.30 In-Process Startup Failure**</span></span>

<span data-ttu-id="971cd-129">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-129">The worker process fails.</span></span> <span data-ttu-id="971cd-130">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="971cd-130">The app doesn't start.</span></span>

<span data-ttu-id="971cd-131">İşlemdeki .NET Core CLR başlatmak gereken ASP.NET Core modülü çalışır, ancak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-131">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="971cd-132">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="971cd-132">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="971cd-133">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="971cd-133">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="971cd-134">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="971cd-134">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="971cd-135">**500.0 işlem içi işleyici yükleme hatası**</span><span class="sxs-lookup"><span data-stu-id="971cd-135">**500.0 In-Process Handler Load Failure**</span></span>

<span data-ttu-id="971cd-136">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-136">The worker process fails.</span></span> <span data-ttu-id="971cd-137">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="971cd-137">The app doesn't start.</span></span>

<span data-ttu-id="971cd-138">.NET Core CLR bulun ve işlemde istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="971cd-138">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="971cd-139">Kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="971cd-139">Check that:</span></span>

* <span data-ttu-id="971cd-140">Uygulamayı ya da hedeflediğinden [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini veya [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="971cd-140">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="971cd-141">ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.</span><span class="sxs-lookup"><span data-stu-id="971cd-141">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

<span data-ttu-id="971cd-142">**500.0 giden işlem işleyicisi yükleme hatası**</span><span class="sxs-lookup"><span data-stu-id="971cd-142">**500.0 Out-Of-Process Handler Load Failure**</span></span>

<span data-ttu-id="971cd-143">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-143">The worker process fails.</span></span> <span data-ttu-id="971cd-144">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="971cd-144">The app doesn't start.</span></span>

<span data-ttu-id="971cd-145">İşlem dışı barındırma istek işleyicisi bulmak gereken ASP.NET Core modülü başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-145">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="971cd-146">Emin *aspnetcorev2_outofprocess.dll* yanında bir alt klasöründe yoksa *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="971cd-146">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span> 

::: moniker-end

<span data-ttu-id="971cd-147">**500 İç sunucu hatası**</span><span class="sxs-lookup"><span data-stu-id="971cd-147">**500 Internal Server Error**</span></span>  
<span data-ttu-id="971cd-148">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="971cd-148">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="971cd-149">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="971cd-149">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="971cd-150">Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="971cd-150">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="971cd-151">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="971cd-151">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="971cd-152">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="971cd-152">From the server's perspective, that's correct.</span></span> <span data-ttu-id="971cd-153">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="971cd-153">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="971cd-154">[Uygulamayı bir komut isteminde aşağıdakini çalıştırın](#run-the-app-at-a-command-prompt) sunucuda veya [ASP.NET Core modülü stdout günlüğünü etkinleştir](#aspnet-core-module-stdout-log) sorunu gidermek için.</span><span class="sxs-lookup"><span data-stu-id="971cd-154">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

<span data-ttu-id="971cd-155">**Bağlantı sıfırlama**</span><span class="sxs-lookup"><span data-stu-id="971cd-155">**Connection reset**</span></span>

<span data-ttu-id="971cd-156">Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda.</span><span class="sxs-lookup"><span data-stu-id="971cd-156">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="971cd-157">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="971cd-157">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="971cd-158">Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata.</span><span class="sxs-lookup"><span data-stu-id="971cd-158">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="971cd-159">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="971cd-159">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="971cd-160">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="971cd-160">Default startup limits</span></span>

<span data-ttu-id="971cd-161">ASP.NET Core modülü ile bir varsayılan yapılandırılmış *startupTimeLimit* 120 saniye.</span><span class="sxs-lookup"><span data-stu-id="971cd-161">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="971cd-162">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="971cd-162">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="971cd-163">Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="971cd-163">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="971cd-164">Uygulama başlatma hatalarını giderme</span><span class="sxs-lookup"><span data-stu-id="971cd-164">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="971cd-165">Uygulama olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="971cd-165">Application Event Log</span></span>

<span data-ttu-id="971cd-166">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="971cd-166">Access the Application Event Log:</span></span>

1. <span data-ttu-id="971cd-167">Başlat menüsünü açın, arama **Olay Görüntüleyicisi'ni**ve ardından **Olay Görüntüleyicisi'ni** uygulama.</span><span class="sxs-lookup"><span data-stu-id="971cd-167">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="971cd-168">İçinde **Olay Görüntüleyicisi'ni**açın **Windows Günlükleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="971cd-168">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="971cd-169">Seçin **uygulama** uygulama olay günlüğünü açın.</span><span class="sxs-lookup"><span data-stu-id="971cd-169">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="971cd-170">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="971cd-170">Search for errors associated with the failing app.</span></span> <span data-ttu-id="971cd-171">Hata içeren bir değeri *IIS AspNetCore Modülü* veya *IIS Express AspNetCore Modülü* içinde *kaynak* sütun.</span><span class="sxs-lookup"><span data-stu-id="971cd-171">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="971cd-172">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="971cd-172">Run the app at a command prompt</span></span>

<span data-ttu-id="971cd-173">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="971cd-173">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="971cd-174">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="971cd-174">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

<span data-ttu-id="971cd-175">**Framework bağımlı dağıtım**</span><span class="sxs-lookup"><span data-stu-id="971cd-175">**Framework-dependent deployment**</span></span>

<span data-ttu-id="971cd-176">Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="971cd-176">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="971cd-177">Bir komut isteminde, dağıtım klasörüne gidin ve uygulama ile uygulamanın derleme yürüterek çalıştırma *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="971cd-177">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="971cd-178">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="971cd-178">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="971cd-179">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="971cd-179">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="971cd-180">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="971cd-180">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="971cd-181">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="971cd-181">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="971cd-182">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun ters proxy yapılandırmasını ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="971cd-182">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

<span data-ttu-id="971cd-183">**Kendi içinde dağıtım**</span><span class="sxs-lookup"><span data-stu-id="971cd-183">**Self-contained deployment**</span></span>

<span data-ttu-id="971cd-184">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="971cd-184">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="971cd-185">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="971cd-185">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="971cd-186">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="971cd-186">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="971cd-187">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="971cd-187">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="971cd-188">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="971cd-188">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="971cd-189">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="971cd-189">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="971cd-190">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun ters proxy yapılandırmasını ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="971cd-190">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the reverse proxy configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="971cd-191">ASP.NET Core modülü stdout günlüğü</span><span class="sxs-lookup"><span data-stu-id="971cd-191">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="971cd-192">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="971cd-192">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="971cd-193">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="971cd-193">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="971cd-194">Varsa *günlükleri* klasör mevcut değilse, bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="971cd-194">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="971cd-195">Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak dağıtım klasörüne bakın [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="971cd-195">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="971cd-196">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="971cd-196">Edit the *web.config* file.</span></span> <span data-ttu-id="971cd-197">Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu işaret edecek şekilde *günlükleri* klasör (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="971cd-197">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="971cd-198">`stdout` Günlük dosyası adı ön eki içinde yoludur.</span><span class="sxs-lookup"><span data-stu-id="971cd-198">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="971cd-199">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="971cd-199">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="971cd-200">Kullanarak `stdout` dosya adı ön eki genel günlük dosyası adında *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="971cd-200">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="971cd-201">Uygulama havuzunun kimliği için yazma izinlerine sahip olduğundan emin olun *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="971cd-201">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="971cd-202">Güncelleştirilmiş Kaydet *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="971cd-202">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="971cd-203">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="971cd-203">Make a request to the app.</span></span>
1. <span data-ttu-id="971cd-204">Gidin *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="971cd-204">Navigate to the *logs* folder.</span></span> <span data-ttu-id="971cd-205">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="971cd-205">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="971cd-206">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="971cd-206">Study the log for errors.</span></span>

<span data-ttu-id="971cd-207">**Önemli!**</span><span class="sxs-lookup"><span data-stu-id="971cd-207">**Important!**</span></span> <span data-ttu-id="971cd-208">Sorun giderme işlemi tamamlandıktan sonra stdout günlüğü devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="971cd-208">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="971cd-209">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="971cd-209">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="971cd-210">Ayarlama **stdoutLogEnabled** için `false`.</span><span class="sxs-lookup"><span data-stu-id="971cd-210">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="971cd-211">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="971cd-211">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="971cd-212">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="971cd-212">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="971cd-213">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="971cd-213">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="971cd-214">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="971cd-214">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="971cd-215">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="971cd-215">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enabling-the-developer-exception-page"></a><span data-ttu-id="971cd-216">Geliştirici özel durum sayfasında etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="971cd-216">Enabling the Developer Exception Page</span></span>

<span data-ttu-id="971cd-217">`ASPNETCORE_ENVIRONMENT` [Ortam değişkeni web.config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) uygulama geliştirme ortamında çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="971cd-217">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="971cd-218">Ortama göre uygulama başlangıç kılmadığınız sürece `UseEnvironment` konak Oluşturucusu'ortam değişkenini ayarlayarak sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesini zaman uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="971cd-218">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="971cd-219">İçin ortam değişkenini ayarlayarak `ASPNETCORE_ENVIRONMENT` yalnızca Internet'e açık olmayan sunucuları test ve hazırlık kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="971cd-219">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="971cd-220">Ortam değişkeninden kaldırmak *web.config* sorun giderme sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="971cd-220">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="971cd-221">Ortam değişkenlerini ayarlama hakkında bilgi *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="971cd-221">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="971cd-222">Ortak başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="971cd-222">Common startup errors</span></span> 

<span data-ttu-id="971cd-223">Bkz. <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="971cd-223">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="971cd-224">Uygulama başlatma önleyen yaygın sorunların çoğunu başvuru konusunda ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="971cd-224">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="971cd-225">Yavaş veya asılı uygulama</span><span class="sxs-lookup"><span data-stu-id="971cd-225">Slow or hanging app</span></span>

<span data-ttu-id="971cd-226">Bir uygulamayı yavaş yanıt ya da bir isteği yanıt vermemeye başlıyor almak ve analiz bir [döküm dosyasını](/visualstudio/debugger/using-dump-files).</span><span class="sxs-lookup"><span data-stu-id="971cd-226">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="971cd-227">Döküm dosyaları aşağıdaki araçlardan herhangi birini kullanarak elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="971cd-227">Dump files can be obtained using any of the following tools:</span></span>

* [<span data-ttu-id="971cd-228">ProcDump</span><span class="sxs-lookup"><span data-stu-id="971cd-228">ProcDump</span></span>](/sysinternals/downloads/procdump)
* [<span data-ttu-id="971cd-229">DebugDiag</span><span class="sxs-lookup"><span data-stu-id="971cd-229">DebugDiag</span></span>](https://www.microsoft.com/download/details.aspx?id=49924)
* <span data-ttu-id="971cd-230">WinDbg: [Windows için hata ayıklama araçları'nı indirin](https://developer.microsoft.com/windows/hardware/download-windbg), [WinDbg kullanarak hata ayıklama](/windows-hardware/drivers/debugger/debugging-using-windbg)</span><span class="sxs-lookup"><span data-stu-id="971cd-230">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="971cd-231">Uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="971cd-231">Remote debugging</span></span>

<span data-ttu-id="971cd-232">Bkz: [Visual Studio 2017'de uzak bir IIS bilgisayarda uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="971cd-232">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="971cd-233">Application Insights</span><span class="sxs-lookup"><span data-stu-id="971cd-233">Application Insights</span></span>

<span data-ttu-id="971cd-234">[Application Insights](/azure/application-insights/) tan alınan telemetri verilerini günlüğe kaydetme ve raporlama özellikleri hata dahil olmak üzere, IIS tarafından barındırılan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="971cd-234">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="971cd-235">Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra gerçekleşen hataları üzerinde rapor edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="971cd-235">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="971cd-236">Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="971cd-236">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-troubleshooting-advice"></a><span data-ttu-id="971cd-237">Ek sorun giderme tavsiyeleri</span><span class="sxs-lookup"><span data-stu-id="971cd-237">Additional troubleshooting advice</span></span>

<span data-ttu-id="971cd-238">Bazen ya da .NET Core SDK içinde uygulama geliştirme makine ya da paket sürümleri üzerinde hemen yükselttikten sonra çalışan bir uygulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="971cd-238">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="971cd-239">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="971cd-239">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="971cd-240">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="971cd-240">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="971cd-241">Silme *bin* ve *obj* klasörleri.</span><span class="sxs-lookup"><span data-stu-id="971cd-241">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="971cd-242">Temizleyin, paketin önbelleğe *% USERPROFILE %\\.nuget\\paketleri* ve *% LocalAppData %\\Nuget\\v3 önbellek*.</span><span class="sxs-lookup"><span data-stu-id="971cd-242">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="971cd-243">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="971cd-243">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="971cd-244">Önceki dağıtım sunucu üzerinde tamamen uygulama dağıtarak önce silindiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="971cd-244">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="971cd-245">Yürütmek için paket önbellekleri temizlemek için kullanışlı bir yol olan `dotnet nuget locals all --clear` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="971cd-245">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
> 
> <span data-ttu-id="971cd-246">Paket önbelleklerini temizleyerek da elde edilebilir kullanarak [nuget.exe](https://www.nuget.org/downloads) araç ve komut yürütme `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="971cd-246">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="971cd-247">*nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen bir yükleme değildir ve gelen ayrı olarak edinilmelidir [NuGet Web sitesi](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="971cd-247">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="971cd-248">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="971cd-248">Additional resources</span></span>

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
