---
title: ASP.NET Core ile Azure App Service ve IIS için ortak hatalar başvurusu
author: guardrex
description: Azure Apps hizmeti ve IIS 'de ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalara yönelik sorun giderme önerisi alın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: f6afd6491181830f4d79486fa26a64423cd4a0ac
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963671"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="bac19-103">ASP.NET Core ile Azure App Service ve IIS için ortak hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="bac19-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="bac19-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bac19-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bac19-105">Bu konuda yaygın hatalar açıklanmakta ve Azure Apps hizmetinde ve IIS 'de ASP.NET Core uygulamalar barındırırken belirli hatalar için sorun giderme önerileri sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bac19-105">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="bac19-106">Genel sorun giderme kılavuzu için bkz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="bac19-106">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="bac19-107">Aşağıdaki bilgileri toplayın:</span><span class="sxs-lookup"><span data-stu-id="bac19-107">Collect the following information:</span></span>

* <span data-ttu-id="bac19-108">Tarayıcı davranışı (durum kodu ve hata iletisi)</span><span class="sxs-lookup"><span data-stu-id="bac19-108">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="bac19-109">Uygulama olay günlüğü girdileri</span><span class="sxs-lookup"><span data-stu-id="bac19-109">Application Event Log entries</span></span>
  * <span data-ttu-id="bac19-110">Azure App Service &ndash; bkz<xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="bac19-110">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="bac19-111">IIS</span><span class="sxs-lookup"><span data-stu-id="bac19-111">IIS</span></span>
    1. <span data-ttu-id="bac19-112">**Windows** menüsünde **Başlat** ' ı seçin, *Olay Görüntüleyicisi*yazın ve **ENTER**tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="bac19-112">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="bac19-113">**Olay Görüntüleyicisi** açıldıktan sonra, kenar çubuğunda **Windows günlükleri** > **uygulaması** ' nı genişletin.</span><span class="sxs-lookup"><span data-stu-id="bac19-113">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="bac19-114">ASP.NET Core modülü stdout ve hata ayıklama günlüğü girdileri</span><span class="sxs-lookup"><span data-stu-id="bac19-114">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="bac19-115">Azure App Service &ndash; bkz<xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="bac19-115">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="bac19-116">IIS &ndash; , ASP.NET Core modülü konusunun [günlük oluşturma ve yeniden yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) ve [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) bölümlerindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-116">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="bac19-117">Hata bilgilerini aşağıdaki yaygın hatalarla karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="bac19-117">Compare error information to the following common errors.</span></span> <span data-ttu-id="bac19-118">Bir eşleşme bulunursa, sorun giderme talimatını izleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-118">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="bac19-119">Bu konudaki hataların listesi ayrıntılı değildir.</span><span class="sxs-lookup"><span data-stu-id="bac19-119">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="bac19-120">Burada listelenmeyen bir hatayla karşılaşırsanız, bu konunun en altındaki **içerik geri bildirim** düğmesini kullanarak yeni bir sorun açın ve hatayı yeniden oluşturma hakkında ayrıntılı yönergeler kullanın.</span><span class="sxs-lookup"><span data-stu-id="bac19-120">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="bac19-121">Yükleyici VC + + yeniden dağıtılabilir alamıyor</span><span class="sxs-lookup"><span data-stu-id="bac19-121">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="bac19-122">**Yükleyici özel durumu:** 0x80072EFD **--veya--** 0x80072F76-belirtilmeyen hata</span><span class="sxs-lookup"><span data-stu-id="bac19-122">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="bac19-123">**Yükleyici günlüğü özel&#8224;durumu:** Hata 0x80072EFD **--veya--** 0x80072F76: EXE paketi yürütülemedi</span><span class="sxs-lookup"><span data-stu-id="bac19-123">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="bac19-124">&#8224;Günlük *C:\Users\{Kullanıcı} \AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="bac19-124">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="bac19-125">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-125">Troubleshooting:</span></span>

<span data-ttu-id="bac19-126">[.NET Core barındırma paketini yüklerken](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)sistemin Internet erişimi yoksa, bu özel durum yükleyicinin  *C++ Microsoft Visual 2015 yeniden dağıtılabilir*sürümünü almasını engellerse oluşur.</span><span class="sxs-lookup"><span data-stu-id="bac19-126">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="bac19-127">[Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=53840)' nden bir yükleyici edinin.</span><span class="sxs-lookup"><span data-stu-id="bac19-127">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="bac19-128">Yükleyici başarısız olursa, sunucu [çerçeveye bağımlı bir dağıtımı (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd)barındırmak için gereken .NET Core çalışma zamanını alamayabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-128">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="bac19-129">FDD barındırıyorsanız, çalışma zamanının **programlar & Özellikler** veya **uygulamalar & Özellikler**' de yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-129">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="bac19-130">Belirli bir çalışma zamanı gerekliyse, [.net download arşivleri](https://dotnet.microsoft.com/download/archives) ' nden çalışma zamanını indirin ve sisteme yükleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-130">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="bac19-131">Çalışma zamanını yükledikten sonra, bir komut isteminden net **stop was/y** ve ardından **net start w3svc** ' i yürüterek SISTEMI yeniden başlatın veya IIS 'yi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="bac19-131">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="bac19-132">İşletim sistemi yükseltmesi 32-bit ASP.NET Core modülünü kaldırdı</span><span class="sxs-lookup"><span data-stu-id="bac19-132">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="bac19-133">**Uygulama günlüğü:** **C:\windows\system32\inetsrv\aspnetcore.dll** modül dll 'si yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="bac19-133">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="bac19-134">Veriler hatadır.</span><span class="sxs-lookup"><span data-stu-id="bac19-134">The data is the error.</span></span>

<span data-ttu-id="bac19-135">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-135">Troubleshooting:</span></span>

<span data-ttu-id="bac19-136">Bir işletim sistemi yükseltmesi sırasında **C:\Windows\SysWOW64\inetsrv** dizininde işletim sistemi olmayan dosyalar korunmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-136">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="bac19-137">ASP.NET Core modülü bir işletim sistemi yükseltmesinden önce yüklendiyse ve sonra herhangi bir uygulama havuzu bir işletim sistemi yükseltmesinden sonra 32 bit modda çalıştıktan sonra bu sorunla karşılaşılmıştır.</span><span class="sxs-lookup"><span data-stu-id="bac19-137">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="bac19-138">Bir işletim sistemi yükseltmesinden sonra ASP.NET Core modülünü onarın.</span><span class="sxs-lookup"><span data-stu-id="bac19-138">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="bac19-139">Bkz. [.NET Core barındırma paketi 'Ni yüklemeyi](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bac19-139">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="bac19-140">Yükleyici çalıştırıldığında **Onar** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="bac19-140">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="bac19-141">Eksik site uzantısı, 32-bit (x86) ve 64-bit (x64) site uzantıları yüklü veya yanlış işlem bit genişliği ayarlanmış</span><span class="sxs-lookup"><span data-stu-id="bac19-141">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="bac19-142">*Azure Uygulama Hizmetleri tarafından barındırılan uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="bac19-142">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="bac19-143">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-143">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="bac19-144">**Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="bac19-144">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bac19-145">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-145">Could not find inprocess request handler.</span></span> <span data-ttu-id="bac19-146">Hostfxr çağırmadan yakalanan çıkış: Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="bac19-146">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="bac19-147">Belirtilen ' Microsoft. aspnetcore. App ' çerçevesi, ' {Version}-Preview-\*' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-147">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="bac19-148">'/LM/W3SVC/1416782824/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.</span><span class="sxs-lookup"><span data-stu-id="bac19-148">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="bac19-149">**ASP.NET Core modülü stdout günlüğü:** Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="bac19-149">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="bac19-150">Belirtilen ' Microsoft. aspnetcore. App ' çerçevesi, ' {Version}-Preview-\*' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-150">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-151">**ASP.NET Core modülü hata ayıklama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="bac19-151">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bac19-152">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-152">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="bac19-153">Başarısız HRESULT döndürüldü: 0x8000FFFF.</span><span class="sxs-lookup"><span data-stu-id="bac19-153">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="bac19-154">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-154">Could not find inprocess request handler.</span></span> <span data-ttu-id="bac19-155">Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="bac19-155">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="bac19-156">Belirtilen ' Microsoft. aspnetcore. App ' çerçevesi, ' {Version}-Preview-\*' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-156">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="bac19-157">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-157">Troubleshooting:</span></span>

* <span data-ttu-id="bac19-158">Uygulamayı bir önizleme çalışma zamanı üzerinde çalıştırıyorsanız, uygulamanın ve uygulamanın çalışma zamanının bit durumuyla eşleşen 32-bit (x86) **veya** 64 bit (x64) site uzantısını da yükler.</span><span class="sxs-lookup"><span data-stu-id="bac19-158">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="bac19-159">**Uzantı veya birden çok çalışma zamanı sürümünü yüklemeyin.**</span><span class="sxs-lookup"><span data-stu-id="bac19-159">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="bac19-160">ASP.NET Core {RUNTIME VERSION} (x86) çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="bac19-160">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="bac19-161">ASP.NET Core {RUNTIME VERSION} (x64) çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="bac19-161">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="bac19-162">Uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="bac19-162">Restart the app.</span></span> <span data-ttu-id="bac19-163">Uygulamanın yeniden başlatılması için birkaç saniye bekleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-163">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="bac19-164">Uygulamayı bir önizleme çalışma zamanında çalıştırmak ve 32-bit (x86) ve 64 bit (x64) [site uzantıları](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) yüklüyse, uygulamanın bit durumuyla eşleşmeyen site uzantısını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="bac19-164">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="bac19-165">Site uzantısını kaldırdıktan sonra uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="bac19-165">After removing the site extension, restart the app.</span></span> <span data-ttu-id="bac19-166">Uygulamanın yeniden başlatılması için birkaç saniye bekleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-166">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="bac19-167">Uygulamayı bir önizleme çalışma zamanında çalıştırmak ve site uzantısının bit kullanımı uygulamayla eşleşiyorsa, önizleme sitesi uzantısının *çalışma zamanı sürümünün* uygulamanın çalışma zamanı sürümüyle eşleştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-167">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="bac19-168">**Uygulamanın uygulama ayarlarındaki** **platformunun** uygulamanın bit durumuyla eşleştiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="bac19-168">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="bac19-169">Daha fazla bilgi için bkz. <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="bac19-169">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="bac19-170">X86 uygulaması dağıtıldı, ancak uygulama havuzu 32-bit uygulamalar için etkinleştirilmemiş</span><span class="sxs-lookup"><span data-stu-id="bac19-170">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="bac19-171">**Tarayıcı:** HTTP hatası 500,30-Işlem Içi Işlem başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-171">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="bac19-172">**Uygulama günlüğü:** ' {PATH} ' fiziksel köküne sahip '/LM/W3SVC/5/ROOT ' uygulaması beklenmeyen yönetilen özel duruma ulaştı, özel durum kodu = ' 0xe0434352 '.</span><span class="sxs-lookup"><span data-stu-id="bac19-172">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="bac19-173">Daha fazla bilgi için lütfen stderr günlüklerine bakın.</span><span class="sxs-lookup"><span data-stu-id="bac19-173">Please check the stderr logs for more information.</span></span> <span data-ttu-id="bac19-174">' {PATH} ' fiziksel köküne sahip '/LM/W3SVC/5/ROOT ' uygulaması clr ve yönetilen uygulamayı yükleyemedi.</span><span class="sxs-lookup"><span data-stu-id="bac19-174">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="bac19-175">CLR Worker iş parçacığından erken çıkıldı</span><span class="sxs-lookup"><span data-stu-id="bac19-175">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="bac19-176">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.</span><span class="sxs-lookup"><span data-stu-id="bac19-176">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-177">**ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürüldü: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="bac19-177">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="bac19-178">Bu senaryo, kendi içinde bulunan bir uygulama yayımlanırken SDK tarafından yakalanarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="bac19-178">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="bac19-179">RID platform hedefi ile eşleşmezse SDK bir hata üretir (örneğin, `win10-x64` proje dosyasında ile `<PlatformTarget>x86</PlatformTarget>` RID).</span><span class="sxs-lookup"><span data-stu-id="bac19-179">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="bac19-180">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-180">Troubleshooting:</span></span>

<span data-ttu-id="bac19-181">X86 çerçevesine bağımlı bir dağıtım (`<PlatformTarget>x86</PlatformTarget>`) için, 32 bitlik uygulamalar için IIS uygulama havuzunu etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="bac19-181">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="bac19-182">IIS Yöneticisi 'nde, uygulama havuzunun **Gelişmiş ayarlarını** açın ve **32 bitlik uygulamaları** **doğru**olarak etkinleştir ayarını yapın.</span><span class="sxs-lookup"><span data-stu-id="bac19-182">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="bac19-183">Platform RID ile çakışıyor</span><span class="sxs-lookup"><span data-stu-id="bac19-183">Platform conflicts with RID</span></span>

* <span data-ttu-id="bac19-184">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-184">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bac19-185">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "C:\{Path} {Assembly} komut satırı ile işlem başlatamadı. { exe | dll} "', ErrorCode = ' 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="bac19-185">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="bac19-186">**ASP.NET Core modülü stdout günlüğü:** İşlenmeyen özel durum: System. BadImageFormatException: ' {ASSEMBLY}. dll ' dosyası veya bütünleştirilmiş kodu yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="bac19-186">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="bac19-187">Bir programı hatalı biçimde yükleme girişiminde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="bac19-187">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="bac19-188">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-188">Troubleshooting:</span></span>

* <span data-ttu-id="bac19-189">Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-189">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bac19-190">İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-190">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bac19-191">Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="bac19-191">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="bac19-192">Bu özel durum, bir uygulamayı yükseltirken ve daha yeni derlemeler dağıtıldığında bir Azure Apps dağıtımı için oluşursa, önceki dağıtımdan tüm dosyaları el ile silin.</span><span class="sxs-lookup"><span data-stu-id="bac19-192">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="bac19-193">Yükseltilmiş bir uygulama dağıtımında, kalan uyumsuz `System.BadImageFormatException` derlemeler bir özel durumla sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-193">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="bac19-194">URI uç noktası yanlış veya durdurulmuş Web sitesi</span><span class="sxs-lookup"><span data-stu-id="bac19-194">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="bac19-195">**Tarayıcı:** ERR_CONNECTION_REFUSED **--veya--** bağlantı kurulamıyor</span><span class="sxs-lookup"><span data-stu-id="bac19-195">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="bac19-196">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="bac19-196">**Application Log:** No entry</span></span>

* <span data-ttu-id="bac19-197">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-197">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-198">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-198">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bac19-199">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-199">Troubleshooting:</span></span>

* <span data-ttu-id="bac19-200">Uygulamanın kullanımda olduğu doğru URI uç noktasını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-200">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="bac19-201">Bağlamaları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-201">Check the bindings.</span></span>

* <span data-ttu-id="bac19-202">IIS Web sitesinin *durdurulmuş* durumda olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-202">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="bac19-203">CoreWebEngine veya W3SVC sunucu özellikleri devre dışı</span><span class="sxs-lookup"><span data-stu-id="bac19-203">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="bac19-204">**İşletim sistemi özel durumu:** ASP.NET Core modülünü kullanmak için IIS 7,0 CoreWebEngine ve W3SVC özelliklerinin yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bac19-204">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="bac19-205">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-205">Troubleshooting:</span></span>

<span data-ttu-id="bac19-206">Uygun rol ve özelliklerin etkinleştirildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-206">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="bac19-207">Bkz. [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bac19-207">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="bac19-208">Yanlış web sitesi fiziksel yolu veya uygulaması eksik</span><span class="sxs-lookup"><span data-stu-id="bac19-208">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="bac19-209">**Tarayıcı:** 403 Yasak-erişim reddedildi **--veya--** 403,14 yasak-Web sunucusu bu dizinin içeriğini listebir şekilde yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="bac19-209">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="bac19-210">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="bac19-210">**Application Log:** No entry</span></span>

* <span data-ttu-id="bac19-211">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-211">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-212">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-212">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bac19-213">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-213">Troubleshooting:</span></span>

<span data-ttu-id="bac19-214">IIS Web sitesi **temel ayarları** ve fiziksel uygulama klasörü ' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="bac19-214">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="bac19-215">Uygulamanın IIS Web sitesi **fiziksel yolundaki**klasörde olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-215">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="bac19-216">Yanlış rol, ASP.NET Core modülü yüklü değil veya yanlış izinler</span><span class="sxs-lookup"><span data-stu-id="bac19-216">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="bac19-217">**Tarayıcı:** 500,19 iç sunucu hatası-sayfa için ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor.</span><span class="sxs-lookup"><span data-stu-id="bac19-217">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="bac19-218">**--Veya--** Bu sayfa görüntülenemiyor</span><span class="sxs-lookup"><span data-stu-id="bac19-218">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="bac19-219">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="bac19-219">**Application Log:** No entry</span></span>

* <span data-ttu-id="bac19-220">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-220">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-221">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-221">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bac19-222">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-222">Troubleshooting:</span></span>

* <span data-ttu-id="bac19-223">Doğru rolün etkin olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-223">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="bac19-224">Bkz. [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bac19-224">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="bac19-225">**Programlar & Özellikler** veya **uygulamalar & özellikleri** açın ve **Windows Server barındırma** 'nın yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-225">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="bac19-226">Yüklü programlar listesinde **Windows Server barındırma** yoksa, .NET Core barındırma paketi ' ni indirip yükleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-226">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="bac19-227">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="bac19-227">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="bac19-228">Daha fazla bilgi için bkz. [.NET Core barındırma paketini yüklemeye](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bac19-228">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="bac19-229">**Uygulama havuzu** > **işlem modeli** > **kimliğinin** **applicationpokaydentity** olarak ayarlandığından veya özel kimliğin uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="bac19-229">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="bac19-230">ASP.NET Core barındırma paketini kaldırdıysanız ve barındırma paketinin önceki bir sürümünü yüklediyseniz *ApplicationHost. config* dosyası ASP.NET Core modülü için bir bölüm içermez.</span><span class="sxs-lookup"><span data-stu-id="bac19-230">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="bac19-231">*ApplicationHost. config* dosyasını *% windir%/system32/inetsrv/config* konumunda açın `<configuration><configSections><sectionGroup name="system.webServer">` ve bölüm grubunu bulun.</span><span class="sxs-lookup"><span data-stu-id="bac19-231">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="bac19-232">Bölüm grubunda ASP.NET Core modülünün bölümü eksikse, Bölüm öğesini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bac19-232">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="bac19-233">Alternatif olarak, ASP.NET Core barındıran paketin en son sürümünü de yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="bac19-233">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="bac19-234">En son sürüm, desteklenen ASP.NET Core uygulamalarla geriye dönük olarak uyumludur.</span><span class="sxs-lookup"><span data-stu-id="bac19-234">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="bac19-235">Hatalı processPath, eksik yol değişkeni, barındırma paketi yüklü değil, sistem/IIS yeniden başlatılmadı, VC + + yeniden dağıtılabilir yüklü değil veya DotNet. exe erişim ihlali</span><span class="sxs-lookup"><span data-stu-id="bac19-235">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-236">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-236">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="bac19-237">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "{...}" komut satırı ile işleme başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="bac19-237">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="bac19-238">', ErrorCode = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="bac19-238">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="bac19-239">' {PATH} ' uygulaması başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-239">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="bac19-240">' {PATH} ' konumunda yürütülebilir dosya bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-240">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="bac19-241">'/LM/W3SVC/2/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8007023e '.</span><span class="sxs-lookup"><span data-stu-id="bac19-241">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="bac19-242">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-242">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="bac19-243">**ASP.NET Core modülü hata ayıklama günlüğü:** Olay günlüğü: ' {PATH} ' uygulaması başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-243">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="bac19-244">' {PATH} ' konumunda yürütülebilir dosya bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-244">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="bac19-245">Başarısız HRESULT döndürüldü: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="bac19-245">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bac19-246">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-246">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bac19-247">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "{...}" komut satırı ile işleme başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="bac19-247">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="bac19-248">', ErrorCode = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="bac19-248">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="bac19-249">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.</span><span class="sxs-lookup"><span data-stu-id="bac19-249">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="bac19-250">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-250">Troubleshooting:</span></span>

* <span data-ttu-id="bac19-251">Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-251">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bac19-252">İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-252">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bac19-253">Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="bac19-253">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="bac19-254">Bir çerçeveye bağımlı dağıtım (FDD `<aspNetCore>` ) veya `.\{ASSEMBLY}.exe` [kendi kendine ait dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)için `dotnet` olduğunu doğrulamak üzere *Web. config* dosyasındaki öğesindeki *processPath* özniteliğini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-254">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="bac19-255">FDD için, *DotNet. exe* ' nin yol ayarları aracılığıyla erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-255">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="bac19-256">*C:\Program files\dotnet\\*  dosyasının sistem yolu ayarlarında bulunduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-256">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="bac19-257">FDD için, *DotNet. exe* ' yi uygulama havuzunun Kullanıcı kimliği için erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-257">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="bac19-258">Uygulama havuzu Kullanıcı kimliğinin *C:\Program Files\dotnet* dizinine erişimi olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-258">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="bac19-259">*C:\Program Files\dotnet* ve uygulama dizinlerindeki uygulama havuzu Kullanıcı kimliği için yapılandırılmış reddetme kuralı olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-259">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="bac19-260">Bir FDD dağıtılmış ve IIS 'nin yeniden başlatılmasına gerek kalmadan .NET Core yüklenmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-260">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="bac19-261">Bir komut isteminden net **stop was/y** ve ardından **net start w3svc** ' i yürüterek sunucuyu YENIDEN başlatın ya da IIS 'yi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="bac19-261">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bac19-262">Bir FDD, barındırma sistemine .NET Core çalışma zamanı yüklenmeden dağıtılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-262">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="bac19-263">.NET Core çalışma zamanı yüklenmemişse, sistemde **.NET Core barındırma paketi yükleyicisini** çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bac19-263">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="bac19-264">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="bac19-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="bac19-265">Daha fazla bilgi için bkz. [.NET Core barındırma paketini yüklemeye](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bac19-265">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="bac19-266">Belirli bir çalışma zamanı gerekliyse, [.net download arşivleri](https://dotnet.microsoft.com/download/archives) ' nden çalışma zamanını indirin ve sisteme yükleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-266">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="bac19-267">Bir komut isteminden net **stop idi** ve ardından **net start w3svc** ' i yürüterek SISTEMI yeniden başlatarak veya IIS 'yi yeniden başlatarak yüklemeyi doldurun.</span><span class="sxs-lookup"><span data-stu-id="bac19-267">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bac19-268">Bir FDD dağıtılmış olabilir ve sistemde *Microsoft Visual C++ 2015 Redistributable (x64)* yüklü değildir.</span><span class="sxs-lookup"><span data-stu-id="bac19-268">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="bac19-269">[Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=53840)' nden bir yükleyici edinin.</span><span class="sxs-lookup"><span data-stu-id="bac19-269">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="bac19-270">\<Aspnetcore > öğesinin bağımsız değişkenleri yanlış</span><span class="sxs-lookup"><span data-stu-id="bac19-270">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-271">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-271">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="bac19-272">**Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="bac19-272">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bac19-273">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-273">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="bac19-274">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-274">Could not find inprocess request handler.</span></span> <span data-ttu-id="bac19-275">Hostfxr çağırmadan yakalanan çıkış: DotNet SDK komutlarını çalıştırmak mı istediniz?</span><span class="sxs-lookup"><span data-stu-id="bac19-275">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="bac19-276">Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 '/LM/W3SVC/3/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.</span><span class="sxs-lookup"><span data-stu-id="bac19-276">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="bac19-277">**ASP.NET Core modülü stdout günlüğü:** DotNet SDK komutlarını çalıştırmak mı istediniz?</span><span class="sxs-lookup"><span data-stu-id="bac19-277">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="bac19-278">Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="bac19-278">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="bac19-279">**ASP.NET Core modülü hata ayıklama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="bac19-279">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bac19-280">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-280">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="bac19-281">Başarısız HRESULT döndürüldü: 0x8000FFFF, InProcess istek işleyicisi bulamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-281">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="bac19-282">Hostfxr çağırmadan yakalanan çıkış: DotNet SDK komutlarını çalıştırmak mı istediniz?</span><span class="sxs-lookup"><span data-stu-id="bac19-282">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="bac19-283">Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Başarısız HRESULT döndürüldü: 0x8000FFFF</span><span class="sxs-lookup"><span data-stu-id="bac19-283">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bac19-284">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-284">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bac19-285">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "DotNet" CommandLine ile işlemi başlatamadı.\{ ASSEMBLY}. dll ', ErrorCode = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="bac19-285">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="bac19-286">**ASP.NET Core modülü stdout günlüğü:** Yürütülecek uygulama yok: ' Yol\{derlemesi}. dll '</span><span class="sxs-lookup"><span data-stu-id="bac19-286">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="bac19-287">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-287">Troubleshooting:</span></span>

* <span data-ttu-id="bac19-288">Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-288">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bac19-289">İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-289">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bac19-290">Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="bac19-290">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="bac19-291">Bir çerçeveye bağımlı dağıtım (FDD) veya (b) yok, boş bir dize`arguments=""`() veya `.\{ASSEMBLY}.dll` bir listesi olduğunu doğrulamak için *Web. config* dosyasındaki `<aspNetCore>` öğesindeki *arguments* özniteliğini inceleyin. kendi içinde bir dağıtım`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`(SCD) için uygulamanın bağımsız değişkenleri ().</span><span class="sxs-lookup"><span data-stu-id="bac19-291">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="bac19-292">Eksik .NET Core paylaşılan çerçevesi</span><span class="sxs-lookup"><span data-stu-id="bac19-292">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="bac19-293">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-293">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="bac19-294">**Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="bac19-294">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bac19-295">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bac19-295">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="bac19-296">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-296">Could not find inprocess request handler.</span></span> <span data-ttu-id="bac19-297">Hostfxr çağırmadan yakalanan çıkış: Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="bac19-297">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="bac19-298">Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION} ' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-298">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="bac19-299">'/LM/W3SVC/5/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.</span><span class="sxs-lookup"><span data-stu-id="bac19-299">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="bac19-300">**ASP.NET Core modülü stdout günlüğü:** Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="bac19-300">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="bac19-301">Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION} ' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-301">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="bac19-302">**ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürüldü: 0x8000FFFF</span><span class="sxs-lookup"><span data-stu-id="bac19-302">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="bac19-303">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-303">Troubleshooting:</span></span>

<span data-ttu-id="bac19-304">Çerçeveye bağımlı bir dağıtım (FDD) için, sistemde doğru çalışma zamanının yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-304">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="bac19-305">Uygulama havuzu durduruldu</span><span class="sxs-lookup"><span data-stu-id="bac19-305">Stopped Application Pool</span></span>

* <span data-ttu-id="bac19-306">**Tarayıcı:** 503 hizmeti kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="bac19-306">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="bac19-307">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="bac19-307">**Application Log:** No entry</span></span>

* <span data-ttu-id="bac19-308">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-308">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-309">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-309">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bac19-310">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-310">Troubleshooting:</span></span>

<span data-ttu-id="bac19-311">Uygulama havuzunun *durdurulmuş* durumda olmadığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bac19-311">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="bac19-312">Alt uygulama bir \<işleyiciler > bölümü içerir</span><span class="sxs-lookup"><span data-stu-id="bac19-312">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="bac19-313">**Tarayıcı:** HTTP hatası 500,19-Iç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-313">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="bac19-314">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="bac19-314">**Application Log:** No entry</span></span>

* <span data-ttu-id="bac19-315">**ASP.NET Core modülü stdout günlüğü:** Kök uygulamanın günlük dosyası oluşturulur ve normal işlemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="bac19-315">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="bac19-316">Alt uygulamanın günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-316">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-317">**ASP.NET Core modülü hata ayıklama günlüğü:** Kök uygulamanın günlük dosyası oluşturulur ve normal işlemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="bac19-317">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="bac19-318">Alt uygulamanın günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-318">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bac19-319">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-319">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="bac19-320">Alt uygulamanın *Web. config* dosyasının bir `<handlers>` bölüm içermediğinden veya alt uygulamanın üst uygulamanın işleyicilerini almadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="bac19-320">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="bac19-321">*Web. config* dosyasının `<system.webServer>` üst uygulamanın bölümü bir `<location>` öğesinin içine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="bac19-321">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="bac19-322">Özelliği, [Konum \<>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi `false` içinde belirtilen ayarların üst uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını belirtmek için olarak ayarlanır. <xref:System.Configuration.SectionInformation.InheritInChildApplications*></span><span class="sxs-lookup"><span data-stu-id="bac19-322">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="bac19-323">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="bac19-323">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="bac19-324">Alt uygulamanın *Web. config* dosyasının bir `<handlers>` bölüm içermediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="bac19-324">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="bac19-325">stdout günlük yolu yanlış</span><span class="sxs-lookup"><span data-stu-id="bac19-325">stdout log path incorrect</span></span>

* <span data-ttu-id="bac19-326">**Tarayıcı:** Uygulama normal olarak yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="bac19-326">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-327">**Uygulama günlüğü:** C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-327">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bac19-328">Özel durum iletisi: {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84 konumunda HRESULT 0x80070005 döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="bac19-328">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="bac19-329">C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi durdurulamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-329">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bac19-330">Özel durum iletisi: HRESULT 0x80070002, {PATH} konumunda döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="bac19-330">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="bac19-331">{PATH} \aspnetcorev2_ınprocess.exe içinde stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-331">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="bac19-332">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-332">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="bac19-333">**ASP.NET Core modülü hata ayıklama günlüğü:** C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-333">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bac19-334">Özel durum iletisi: {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84 konumunda HRESULT 0x80070005 döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="bac19-334">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="bac19-335">C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi durdurulamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-335">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bac19-336">Özel durum iletisi: HRESULT 0x80070002, {PATH} konumunda döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="bac19-336">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="bac19-337">{PATH} \aspnetcorev2_ınprocess.exe içinde stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-337">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bac19-338">**Uygulama günlüğü:** Uyarı: StdoutLogFile \\oluşturulamadı mi?\{ YOL} \path_doesnt_exist\stdout_{PROCESS ID} _ {TIMESTAMP}. log, ErrorCode =-2147024893.</span><span class="sxs-lookup"><span data-stu-id="bac19-338">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="bac19-339">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="bac19-339">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bac19-340">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-340">Troubleshooting:</span></span>

* <span data-ttu-id="bac19-341">*Web.* config `<aspNetCore>` öğesinin öğesinde belirtilen yolyok.`stdoutLogFile`</span><span class="sxs-lookup"><span data-stu-id="bac19-341">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="bac19-342">Daha fazla bilgi için bkz [. asp.NET Core modülü: Günlük oluşturma ve yeniden](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="bac19-342">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="bac19-343">Uygulama havuzu kullanıcısının stdout günlük yoluna yazma erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="bac19-343">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="bac19-344">Uygulama yapılandırması genel sorunu</span><span class="sxs-lookup"><span data-stu-id="bac19-344">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bac19-345">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası **--veya--** HTTP hatası 500,30-Ancm Işlem Içi başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-345">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="bac19-346">**Uygulama günlüğü:** Değişken</span><span class="sxs-lookup"><span data-stu-id="bac19-346">**Application Log:** Variable</span></span>

* <span data-ttu-id="bac19-347">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boş veya, uygulamanın noktası başarısız olana kadar normal girdilerle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bac19-347">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="bac19-348">**ASP.NET Core modülü hata ayıklama günlüğü:** Değişken</span><span class="sxs-lookup"><span data-stu-id="bac19-348">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bac19-349">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bac19-349">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bac19-350">**Uygulama günlüğü:** Fiziksel kökü\{' c: Path}\' olan ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, CommandLine ' "c:\{Path}\{Assembly} ile oluşturulmuş işlem. { exe | dll} "', ancak belirtilen ' {PORT} ' bağlantı noktasında kilitlendi veya yanıt vermedi ya da bu bağlantı noktası üzerinde dinleme yapamadı, ErrorCode = ' {ERROR CODE} '</span><span class="sxs-lookup"><span data-stu-id="bac19-350">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="bac19-351">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.</span><span class="sxs-lookup"><span data-stu-id="bac19-351">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="bac19-352">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="bac19-352">Troubleshooting:</span></span>

<span data-ttu-id="bac19-353">Büyük olasılıkla uygulama yapılandırması veya programlama sorunu nedeniyle işlem başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="bac19-353">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="bac19-354">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="bac19-354">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
