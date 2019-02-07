---
title: Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu
author: guardrex
description: Azure uygulama hizmeti ve IIS üzerinde ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalar için sorun giderme tavsiyeleri edinin.
ms.author: riande
ms.custom: mvc
ms.date: 02/05/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 976f7e3fbeab9e81ba99e2dd7d09a892b854651b
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854467"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="ea96f-103">Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="ea96f-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="ea96f-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ea96f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ea96f-105">Bu konuda, Azure uygulama hizmeti ve IIS üzerinde ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalar için sorun giderme tavsiyeleri sunar.</span><span class="sxs-lookup"><span data-stu-id="ea96f-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="ea96f-106">Aşağıdaki bilgileri toplayın:</span><span class="sxs-lookup"><span data-stu-id="ea96f-106">Collect the following information:</span></span>

* <span data-ttu-id="ea96f-107">Tarayıcı davranışı (durum kodu ve hata iletisi)</span><span class="sxs-lookup"><span data-stu-id="ea96f-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="ea96f-108">Uygulama olay günlüğü girişleri</span><span class="sxs-lookup"><span data-stu-id="ea96f-108">Application Event Log entries</span></span>
  * <span data-ttu-id="ea96f-109">Azure App Service'e &ndash; bkz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="ea96f-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="ea96f-110">IIS</span><span class="sxs-lookup"><span data-stu-id="ea96f-110">IIS</span></span>
    1. <span data-ttu-id="ea96f-111">Seçin **Başlat** üzerinde **Windows** menüsü, türü *Olay Görüntüleyicisi'ni*basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="ea96f-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="ea96f-112">Sonra **Olay Görüntüleyicisi'ni** açıldığında genişletin **Windows Günlükleri** > **uygulama** Kenar çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="ea96f-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="ea96f-113">ASP.NET Core modülü stdout ve hata ayıklama günlük girişleri</span><span class="sxs-lookup"><span data-stu-id="ea96f-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="ea96f-114">Azure App Service'e &ndash; bkz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="ea96f-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="ea96f-115">IIS &ndash; yönergeleri [günlük oluşturma ve yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) ve [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) ASP.NET Core modülü konunun bölümleri.</span><span class="sxs-lookup"><span data-stu-id="ea96f-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="ea96f-116">Aşağıdaki sık karşılaşılan hata bilgilerini karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="ea96f-117">Bir eşleşme bulunursa, sorun giderme tavsiyeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="ea96f-118">Bu konudaki hataların listesi kapsamlı değildir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="ea96f-119">Burada listelenmeyen bir hatayla karşılaşırsanız, kullanarak yeni bir sorun açın **içerik geri bildirimi** hatayı yeniden oluşturmaya ilişkin ayrıntılı yönergeler içeren bu konunun sonundaki düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="ea96f-120">Yükleyici VC ++ yeniden dağıtılabilir alamadı</span><span class="sxs-lookup"><span data-stu-id="ea96f-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="ea96f-121">**Yükleyici özel durum:** 0x80072EFD **--veya--** 0x80072f76 - belirtilmeyen hata</span><span class="sxs-lookup"><span data-stu-id="ea96f-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="ea96f-122">**Yükleyici günlük özel durum&#8224;:** Hata 0x80072efd **--veya--** 0x80072f76: EXE paket yürütülemedi</span><span class="sxs-lookup"><span data-stu-id="ea96f-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="ea96f-123">&#8224;Günlük şu konumdadır *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span><span class="sxs-lookup"><span data-stu-id="ea96f-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="ea96f-124">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-124">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-125">Sistem, Internet erişimi yoksa, [.NET Core barındırma paket yükleme](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), yükleyici elde edilen engellendiğinde, bu özel durum oluştu *Microsoft Visual C++ 2015 yeniden dağıtılabilir*.</span><span class="sxs-lookup"><span data-stu-id="ea96f-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="ea96f-126">Bir Yükleyicisi'nden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="ea96f-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="ea96f-127">Yükleyici başarısız olursa sunucunun barındırmak için gereken .NET Core çalışma zamanı almayabilir bir [framework bağımlı dağıtım (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="ea96f-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="ea96f-128">Bir FDD barındırma, çalışma zamanı içinde yüklü olduğunu onaylayın **programlar ve Özellikler** veya **uygulamalar ve Özellikler**.</span><span class="sxs-lookup"><span data-stu-id="ea96f-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="ea96f-129">Belirli bir çalışma zamanı gerekiyorsa, çalışma zamanını şuradan indirin [.NET indirme arşivleri](https://dotnet.microsoft.com/download/archives) ve sisteme yükleyin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="ea96f-130">Çalışma zamanını yükledikten sonra sistemi yeniden başlatın veya yürüterek IIS'yi yeniden **net stop olan /y** ardından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="ea96f-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="ea96f-131">İşletim sistemi yükseltme 32-bit ASP.NET Core modülü kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="ea96f-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="ea96f-132">**Uygulama günlüğü:** Modülü DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="ea96f-133">Veride hata yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="ea96f-133">The data is the error.</span></span>

<span data-ttu-id="ea96f-134">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-134">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-135">Olmayan işletim sistemi dosyaları **C:\Windows\SysWOW64\inetsrv** dizin korunur olmayan bir işletim sistemi yükseltme.</span><span class="sxs-lookup"><span data-stu-id="ea96f-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="ea96f-136">Öncesinde ASP.NET Core Modülü yüklü bir işletim sistemi yükseltmesi ve ardından bir uygulama havuzu çalıştırılır 32-bit modunda işletim sistemi yükseltme sonrasında, bu sorunla karşılaştık.</span><span class="sxs-lookup"><span data-stu-id="ea96f-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="ea96f-137">Bir işletim sistemi yükseltme sonrasında ASP.NET Core Modülü'nü onarın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="ea96f-138">Bkz: [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="ea96f-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="ea96f-139">Seçin **onarım** yükleyici ne zaman çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="ea96f-139">Select **Repair** when the installer is run.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="ea96f-140">Uygulamanın dağıtıldığı bir x86 ancak uygulama havuzunun 32-bit uygulamalar için etkin değil</span><span class="sxs-lookup"><span data-stu-id="ea96f-140">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="ea96f-141">**Tarayıcı:** HTTP Hatası 500.30 - ANCM işlem içi başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-141">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="ea96f-142">**Uygulama günlüğü:** Uygulama '/ LM/W3SVC/5/ROOT' fiziksel ile beklenmeyen yönetilen özel durum, özel durum kodu '{PATH}' kök isabet = '0xe0434352'.</span><span class="sxs-lookup"><span data-stu-id="ea96f-142">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="ea96f-143">Daha fazla bilgi için stderr günlüklerini gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-143">Please check the stderr logs for more information.</span></span> <span data-ttu-id="ea96f-144">Uygulama '/ LM/W3SVC/5/ROOT' ile fiziksel kök '{PATH}' clr ve yönetilen uygulama yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-144">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="ea96f-145">CLR çalışan iş parçacığı beklenenden önce çıkıldı</span><span class="sxs-lookup"><span data-stu-id="ea96f-145">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="ea96f-146">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturuldu, ancak boş olur.</span><span class="sxs-lookup"><span data-stu-id="ea96f-146">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-147">**ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürdü: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="ea96f-147">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="ea96f-148">Bu senaryo, kendi içinde bir uygulama yayımlama sırasında SDK tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="ea96f-148">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="ea96f-149">Platform hedefi RID eşleşmiyorsa SDK'sı bir hata oluşturur. (örneğin, `win10-x64` ile RID `<PlatformTarget>x86</PlatformTarget>` proje dosyasında).</span><span class="sxs-lookup"><span data-stu-id="ea96f-149">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="ea96f-150">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-150">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-151">X x86 için framework bağımlı dağıtım (`<PlatformTarget>x86</PlatformTarget>`), 32-bit uygulamalar için IIS uygulama havuzu etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-151">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="ea96f-152">IIS Yöneticisi'nde açın ve uygulama havuzun **Gelişmiş ayarlar** ve **etkinleştirme 32-Bit uygulamaları** için **True**.</span><span class="sxs-lookup"><span data-stu-id="ea96f-152">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="ea96f-153">RID Platform çakışıyor</span><span class="sxs-lookup"><span data-stu-id="ea96f-153">Platform conflicts with RID</span></span>

* <span data-ttu-id="ea96f-154">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-154">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ea96f-155">**Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "C:\{yolu} {DERLEMESİ}. { exe | dll} "', ErrorCode = ' 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="ea96f-155">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="ea96f-156">**ASP.NET Core modülü stdout günlüğü:** İşlenmeyen özel durum: : System.BadImageFormatException '{} DERLEMESİ .dll' dosya veya derleme yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-156">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="ea96f-157">Bir programı hatalı biçimde yüklemek için girişimde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="ea96f-157">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="ea96f-158">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-158">Troubleshooting:</span></span>

* <span data-ttu-id="ea96f-159">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-159">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="ea96f-160">Bir işlem hatası sonucu uygulama içinde ilgili bir sorun olabilir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-160">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="ea96f-161">Daha fazla bilgi için [sorun giderme (IIS)](xref:host-and-deploy/iis/troubleshoot) veya [sorun giderme (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="ea96f-161">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="ea96f-162">Bu özel durum için bir Azure uygulama dağıtımı bir uygulama yükseltme sırasında gerçekleşir ve yeni derlemeleri dağıtma el ile tüm dosyaları önceki silebilir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-162">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="ea96f-163">Uyumsuz derlemeleri kalan sonuçlanabilir bir `System.BadImageFormatException` yükseltilmiş bir uygulama dağıtımı sırasında özel durum.</span><span class="sxs-lookup"><span data-stu-id="ea96f-163">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="ea96f-164">URI uç nokta yanlış veya durdurulmuş Web sitesi</span><span class="sxs-lookup"><span data-stu-id="ea96f-164">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="ea96f-165">**Tarayıcı:** ERR_CONNECTION_REFUSED **--veya--** bağlanılamıyor</span><span class="sxs-lookup"><span data-stu-id="ea96f-165">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="ea96f-166">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="ea96f-166">**Application Log:** No entry</span></span>

* <span data-ttu-id="ea96f-167">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-167">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-168">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-168">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-169">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-169">Troubleshooting:</span></span>

* <span data-ttu-id="ea96f-170">Uygulama için doğru URI uç nokta kullanımda olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-170">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="ea96f-171">Bağlamaları kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-171">Check the bindings.</span></span>

* <span data-ttu-id="ea96f-172">IIS Web sitesinin içinde olmadığını onaylayın *durduruldu* durumu.</span><span class="sxs-lookup"><span data-stu-id="ea96f-172">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="ea96f-173">Devre dışı CoreWebEngine veya W3SVC sunucusu özellikleri</span><span class="sxs-lookup"><span data-stu-id="ea96f-173">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="ea96f-174">**İşletim sistemi özel durum:** ASP.NET Core modülü kullanmak için IIS 7.0 CoreWebEngine ve W3SVC özelliklerini yüklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-174">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="ea96f-175">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-175">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-176">Uygun rol ve özellikleri etkinleştirildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-176">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="ea96f-177">Bkz: [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="ea96f-177">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="ea96f-178">Yanlış bir Web sitesi fiziksel yolunu veya uygulama eksik</span><span class="sxs-lookup"><span data-stu-id="ea96f-178">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="ea96f-179">**Tarayıcı:** 403 Yasak - erişim reddedildi **--veya--** 403.14 Yasak - Web sunucusu bu dizinin içeriklerini listesinde olmayan şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="ea96f-179">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="ea96f-180">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="ea96f-180">**Application Log:** No entry</span></span>

* <span data-ttu-id="ea96f-181">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-181">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-182">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-182">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-183">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-183">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-184">IIS Web sitesini denetleyin **temel ayarları** ve fiziksel uygulaması klasörü.</span><span class="sxs-lookup"><span data-stu-id="ea96f-184">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="ea96f-185">Uygulama IIS Web sitesinde bir klasör olduğunu onaylayın **fiziksel yolu**.</span><span class="sxs-lookup"><span data-stu-id="ea96f-185">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="ea96f-186">Hatalı bir rol, ASP.NET Core Modülü yüklü değil veya yanlış izinler</span><span class="sxs-lookup"><span data-stu-id="ea96f-186">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="ea96f-187">**Tarayıcı:** 500.19 iç sunucu hatası: sayfa için ilgili yapılandırma verileri geçersiz olduğundan istenen sayfayı erişilemez.</span><span class="sxs-lookup"><span data-stu-id="ea96f-187">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="ea96f-188">**--VEYA--** bu sayfa görüntülenemiyor</span><span class="sxs-lookup"><span data-stu-id="ea96f-188">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="ea96f-189">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="ea96f-189">**Application Log:** No entry</span></span>

* <span data-ttu-id="ea96f-190">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-190">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-191">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-191">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-192">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-192">Troubleshooting:</span></span>

* <span data-ttu-id="ea96f-193">Uygun rol etkin olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-193">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="ea96f-194">Bkz: [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="ea96f-194">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="ea96f-195">Açık **programlar ve Özellikler** veya **uygulamalar ve Özellikler** doğrulayın **Windows Server'ı barındıran** yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-195">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="ea96f-196">Varsa **Windows Server'ı barındıran** yüklü programlar, indirme ve yükleme .NET Core barındırma paket listesinde bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-196">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="ea96f-197">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="ea96f-197">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="ea96f-198">Daha fazla bilgi için [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="ea96f-198">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="ea96f-199">Emin olun **uygulama havuzu** > **işlem modeli** > **kimlik** ayarlanır **ApplicationPoolIdentity** veya özel kimlik uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="ea96f-199">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="ea96f-200">Yanlış processPath, eksik PATH değişkenine, yüklü paket barındırma, yeniden system/IIS, VC ++ yüklü yeniden dağıtılabilir veya dotnet.exe erişim ihlali</span><span class="sxs-lookup"><span data-stu-id="ea96f-200">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-201">**Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-201">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="ea96f-202">**Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "{...}"</span><span class="sxs-lookup"><span data-stu-id="ea96f-202">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="ea96f-203">', ErrorCode = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="ea96f-203">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="ea96f-204">'{PATH}' uygulama başlatmanız mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-204">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="ea96f-205">Yürütülebilir dosya '{PATH}' bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-205">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="ea96f-206">Uygulama başlatılamadı. '/ LM/W3SVC/2/ROOT', '0x8007023e' hata kodu.</span><span class="sxs-lookup"><span data-stu-id="ea96f-206">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="ea96f-207">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-207">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="ea96f-208">**ASP.NET Core modülü hata ayıklama günlüğü:** Olay günlüğü: ' Uygulama '{PATH}' başlatmanız mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-208">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="ea96f-209">Yürütülebilir dosya '{PATH}' bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-209">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="ea96f-210">Başarısız HRESULT döndürdü: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="ea96f-210">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ea96f-211">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-211">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ea96f-212">**Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "{...}"</span><span class="sxs-lookup"><span data-stu-id="ea96f-212">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="ea96f-213">', ErrorCode = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="ea96f-213">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="ea96f-214">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturuldu, ancak boş olur.</span><span class="sxs-lookup"><span data-stu-id="ea96f-214">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-215">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-215">Troubleshooting:</span></span>

* <span data-ttu-id="ea96f-216">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-216">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="ea96f-217">Bir işlem hatası sonucu uygulama içinde ilgili bir sorun olabilir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-217">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="ea96f-218">Daha fazla bilgi için [sorun giderme (IIS)](xref:host-and-deploy/iis/troubleshoot) veya [sorun giderme (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="ea96f-218">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="ea96f-219">Denetleme *processPath* özniteliği `<aspNetCore>` öğesinde *web.config* olduğundan emin olmak için `dotnet` framework bağımlı dağıtım (FDD) veya `.\{ASSEMBLY}.exe` bir için[müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="ea96f-219">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="ea96f-220">Bir FDD için *dotnet.exe* yol ayarları erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-220">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="ea96f-221">Onaylayın *C:\Program Files\dotnet\\*  sistem yolu ayarlarında yok.</span><span class="sxs-lookup"><span data-stu-id="ea96f-221">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="ea96f-222">Bir FDD için *dotnet.exe* uygulama havuzu kullanıcı kimliği için erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-222">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="ea96f-223">Uygulama havuzu kullanıcı kimliği için erişimi olduğunu doğrulamak *C:\Program Files\dotnet* dizin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-223">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="ea96f-224">Uygulama havuzu kullanıcı kimliğini için yapılandırılmış hiçbir Reddet kural onaylayın *C:\Program Files\dotnet* ve uygulama dizinler.</span><span class="sxs-lookup"><span data-stu-id="ea96f-224">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="ea96f-225">Bir FDD dağıtılan ve .NET Core IIS yeniden başlatmanıza gerek kalmadan yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-225">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="ea96f-226">Sunucuyu yeniden başlatın veya yürüterek IIS'yi yeniden **net stop olan /y** ardından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="ea96f-226">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="ea96f-227">Bir FDD barındıran sistemde .NET Core çalışma zamanı yüklemeden dağıtılmış.</span><span class="sxs-lookup"><span data-stu-id="ea96f-227">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="ea96f-228">.NET Core çalışma zamanı yüklü olmadığından çalıştırırsanız **.NET Core barındırma Paket Yükleyici** sistem üzerinde.</span><span class="sxs-lookup"><span data-stu-id="ea96f-228">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="ea96f-229">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="ea96f-229">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="ea96f-230">Daha fazla bilgi için [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="ea96f-230">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="ea96f-231">Belirli bir çalışma zamanı gerekiyorsa, çalışma zamanını şuradan indirin [.NET indirme arşivleri](https://dotnet.microsoft.com/download/archives) ve sisteme yükleyin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-231">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="ea96f-232">Sistem başlatarak veya yürüterek IIS'yi yeniden başlatmak ve yüklemeyi tamamlamak **net stop olan /y** ardından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="ea96f-232">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="ea96f-233">Bir FDD dağıtılan ve *Microsoft Visual C++ 2015 yeniden dağıtılabilir (x64)* sistemde yüklü değil.</span><span class="sxs-lookup"><span data-stu-id="ea96f-233">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="ea96f-234">Bir Yükleyicisi'nden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="ea96f-234">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="ea96f-235">Hatalı bağımsız değişkenleri \<aspNetCore > öğesi</span><span class="sxs-lookup"><span data-stu-id="ea96f-235">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-236">**Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-236">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="ea96f-237">**Uygulama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor.</span><span class="sxs-lookup"><span data-stu-id="ea96f-237">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ea96f-238">Uygulama hatalı yapılandırılmış, büyük olasılıkla bunun anlamı uygulama tarafından hedeflenen ve makinede yüklü sürümler Microsoft.NetCore.App ve Microsoft.AspNetCore.App gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-238">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="ea96f-239">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-239">Could not find inprocess request handler.</span></span> <span data-ttu-id="ea96f-240">Hostfxr çağırma gelen yakalanan çıktısı: Dotnet SDK komutları çalıştırmak mı amaçlamıştınız?</span><span class="sxs-lookup"><span data-stu-id="ea96f-240">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="ea96f-241">Lütfen dotnet SDK'sını yükleyin gelen: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Uygulama başlatılamadı. '/ LM/W3SVC/3/ROOT', '0x8000ffff' hata kodu.</span><span class="sxs-lookup"><span data-stu-id="ea96f-241">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="ea96f-242">**ASP.NET Core modülü stdout günlüğü:** Dotnet SDK komutları çalıştırmak mı amaçlamıştınız?</span><span class="sxs-lookup"><span data-stu-id="ea96f-242">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="ea96f-243">Lütfen dotnet SDK'sını yükleyin gelen: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="ea96f-243">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="ea96f-244">**ASP.NET Core modülü hata ayıklama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor.</span><span class="sxs-lookup"><span data-stu-id="ea96f-244">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ea96f-245">Uygulama hatalı yapılandırılmış, büyük olasılıkla bunun anlamı uygulama tarafından hedeflenen ve makinede yüklü sürümler Microsoft.NetCore.App ve Microsoft.AspNetCore.App gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-245">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="ea96f-246">Başarısız HRESULT döndürdü: 0x8000ffff InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-246">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="ea96f-247">Hostfxr çağırma gelen yakalanan çıktısı: Dotnet SDK komutları çalıştırmak mı amaçlamıştınız?</span><span class="sxs-lookup"><span data-stu-id="ea96f-247">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="ea96f-248">Lütfen dotnet SDK'sını yükleyin gelen: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Başarısız HRESULT döndürdü: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="ea96f-248">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ea96f-249">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-249">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ea96f-250">**Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' ile komut satırı işlemi başlatılamadı ' "dotnet".\{ DERLEME} .dll ', ErrorCode = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="ea96f-250">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="ea96f-251">**ASP.NET Core modülü stdout günlüğü:** Uygulamayı çalıştırmak için mevcut değil: Yolu\{derleme} .dll '</span><span class="sxs-lookup"><span data-stu-id="ea96f-251">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="ea96f-252">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-252">Troubleshooting:</span></span>

* <span data-ttu-id="ea96f-253">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-253">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="ea96f-254">Bir işlem hatası sonucu uygulama içinde ilgili bir sorun olabilir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-254">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="ea96f-255">Daha fazla bilgi için [sorun giderme (IIS)](xref:host-and-deploy/iis/troubleshoot) veya [sorun giderme (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="ea96f-255">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="ea96f-256">İnceleme *bağımsız değişkenleri* özniteliği `<aspNetCore>` öğesinde *web.config* ya da olduğunu onaylamak için (a) `.\{ASSEMBLY}.dll` framework bağımlı dağıtım (FDD); veya (b) yoksa, bir boş dize (`arguments=""`), veya bir uygulamanın bağımsız değişkenleri listesi (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) için kendi içinde bir dağıtım (SCD).</span><span class="sxs-lookup"><span data-stu-id="ea96f-256">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="ea96f-257">.NET Core paylaşılan çerçeve eksik</span><span class="sxs-lookup"><span data-stu-id="ea96f-257">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="ea96f-258">**Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-258">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="ea96f-259">**Uygulama günlüğü:** Yerel bağımlılıkları bulmadan Başarısız InProcess istek işleyicisi bulmak için hostfxr çağrılıyor.</span><span class="sxs-lookup"><span data-stu-id="ea96f-259">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="ea96f-260">Uygulama hatalı yapılandırılmış, büyük olasılıkla bunun anlamı uygulama tarafından hedeflenen ve makinede yüklü sürümler Microsoft.NetCore.App ve Microsoft.AspNetCore.App gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="ea96f-260">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="ea96f-261">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-261">Could not find inprocess request handler.</span></span> <span data-ttu-id="ea96f-262">Hostfxr çağırma gelen yakalanan çıktısı: Tüm uyumlu çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-262">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="ea96f-263">Belirtilen çerçeve 'Microsoft.AspNetCore.App', '{VERSION} sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-263">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="ea96f-264">Uygulama başlatılamadı. '/ LM/W3SVC/5/ROOT', '0x8000ffff' hata kodu.</span><span class="sxs-lookup"><span data-stu-id="ea96f-264">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="ea96f-265">**ASP.NET Core modülü stdout günlüğü:** Tüm uyumlu çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-265">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="ea96f-266">Belirtilen çerçeve 'Microsoft.AspNetCore.App', '{VERSION} sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-266">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="ea96f-267">**ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürdü: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="ea96f-267">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="ea96f-268">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-268">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-269">Framework bağımlı dağıtım (FDD), doğru çalışma zamanı için sistemde yüklü olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="ea96f-269">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="ea96f-270">Durdurulan uygulama havuzunu</span><span class="sxs-lookup"><span data-stu-id="ea96f-270">Stopped Application Pool</span></span>

* <span data-ttu-id="ea96f-271">**Tarayıcı:** 503 Hizmet kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="ea96f-271">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="ea96f-272">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="ea96f-272">**Application Log:** No entry</span></span>

* <span data-ttu-id="ea96f-273">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-273">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-274">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-274">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-275">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-275">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-276">Uygulama havuzu içinde olmadığını onaylayın *durduruldu* durumu.</span><span class="sxs-lookup"><span data-stu-id="ea96f-276">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="ea96f-277">Alt uygulama içeren bir \<işleyicileri > bölümü</span><span class="sxs-lookup"><span data-stu-id="ea96f-277">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="ea96f-278">**Tarayıcı:** HTTP Hatası 500.19 - iç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-278">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="ea96f-279">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="ea96f-279">**Application Log:** No entry</span></span>

* <span data-ttu-id="ea96f-280">**ASP.NET Core modülü stdout günlüğü:** Kök uygulama günlük dosyası oluşturulur ve normal işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-280">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="ea96f-281">Sub uygulamanın günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-281">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-282">**ASP.NET Core modülü hata ayıklama günlüğü:** Kök uygulama günlük dosyası oluşturulur ve normal işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-282">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="ea96f-283">Sub uygulamanın günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-283">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-284">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-284">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ea96f-285">Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölüm veya alt uygulama üst uygulamanın işleyicileri devralmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-285">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="ea96f-286">Üst uygulamanın `<system.webServer>` bölümünü *web.config* içine yerleştirilen bir `<location>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="ea96f-286">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="ea96f-287"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan üst uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="ea96f-287">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="ea96f-288">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="ea96f-288">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ea96f-289">Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="ea96f-289">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="ea96f-290">STDOUT günlük yolu yanlış</span><span class="sxs-lookup"><span data-stu-id="ea96f-290">stdout log path incorrect</span></span>

* <span data-ttu-id="ea96f-291">**Tarayıcı:** Uygulama normal şekilde yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="ea96f-291">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-292">**Uygulama günlüğü:** STDOUT yeniden yönlendirme C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-292">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ea96f-293">Özel durum iletisi: {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. döndürülen HRESULT 0x80070005</span><span class="sxs-lookup"><span data-stu-id="ea96f-293">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="ea96f-294">STDOUT yeniden yönlendirmeyi C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll durdurulamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-294">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ea96f-295">Özel durum iletisi: HRESULT 0x80070002 {PATH} döndürdü.</span><span class="sxs-lookup"><span data-stu-id="ea96f-295">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="ea96f-296">{PATH}\aspnetcorev2_inprocess.dll. yeniden yönlendirme STDOUT başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="ea96f-296">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="ea96f-297">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-297">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="ea96f-298">**ASP.NET Core modülü hata ayıklama günlüğü:** STDOUT yeniden yönlendirme C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-298">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ea96f-299">Özel durum iletisi: {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. döndürülen HRESULT 0x80070005</span><span class="sxs-lookup"><span data-stu-id="ea96f-299">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="ea96f-300">STDOUT yeniden yönlendirmeyi C:\Program Files\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll durdurulamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-300">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="ea96f-301">Özel durum iletisi: HRESULT 0x80070002 {PATH} döndürdü.</span><span class="sxs-lookup"><span data-stu-id="ea96f-301">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="ea96f-302">{PATH}\aspnetcorev2_inprocess.dll. yeniden yönlendirme STDOUT başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="ea96f-302">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ea96f-303">**Uygulama günlüğü:** Uyarı: StdoutLogFile oluşturulamadı \\?\{ ErrorCode yolu} \path_doesnt_exist\stdout_ {işlem kimliği} _ {zaman damgası} .log-2147024893 =.</span><span class="sxs-lookup"><span data-stu-id="ea96f-303">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="ea96f-304">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="ea96f-304">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-305">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-305">Troubleshooting:</span></span>

* <span data-ttu-id="ea96f-306">`stdoutLogFile` Belirtilen yola `<aspNetCore>` öğesinin *web.config* yok.</span><span class="sxs-lookup"><span data-stu-id="ea96f-306">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="ea96f-307">Daha fazla bilgi için [ASP.NET Core Modülü: Günlük oluşturma ve yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="ea96f-307">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="ea96f-308">Uygulama havuzu kullanıcısı stdout günlük yolu yazma erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="ea96f-308">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="ea96f-309">Uygulama yapılandırma genel sorunu</span><span class="sxs-lookup"><span data-stu-id="ea96f-309">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea96f-310">**Tarayıcı:** HTTP Hatası 500.0 - ANCM işlem içi işleyici yükleme hatası **--veya--** HTTP Hatası 500.30 - ANCM işlem içi başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-310">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="ea96f-311">**Uygulama günlüğü:** Değişken</span><span class="sxs-lookup"><span data-stu-id="ea96f-311">**Application Log:** Variable</span></span>

* <span data-ttu-id="ea96f-312">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası uygulama başarısız kadar oluşturulmuş ancak boş veya normal girişleri ile oluşturulan noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="ea96f-312">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="ea96f-313">**ASP.NET Core modülü hata ayıklama günlüğü:** Değişken</span><span class="sxs-lookup"><span data-stu-id="ea96f-313">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ea96f-314">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="ea96f-314">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ea96f-315">**Uygulama günlüğü:** Uygulama ' makine/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' işlemi komut satırı ile oluşturulan ' "C:\{yolu}\{derleme}. { exe | dll} "' ancak arda veya yanıt vermediğinden ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil = '{hata kodu}'</span><span class="sxs-lookup"><span data-stu-id="ea96f-315">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="ea96f-316">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturuldu, ancak boş olur.</span><span class="sxs-lookup"><span data-stu-id="ea96f-316">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="ea96f-317">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="ea96f-317">Troubleshooting:</span></span>

<span data-ttu-id="ea96f-318">İşlem, büyük olasılıkla bir uygulama yapılandırma veya programlama sorunu nedeniyle başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="ea96f-318">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="ea96f-319">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="ea96f-319">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
