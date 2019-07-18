---
title: ASP.NET Core ile Azure App Service ve IIS için ortak hatalar başvurusu
author: guardrex
description: Azure Apps hizmeti ve IIS 'de ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalara yönelik sorun giderme önerisi alın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 3030bc57be113d9034123c96403742442b9240bb
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308096"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="5b63c-103">ASP.NET Core ile Azure App Service ve IIS için ortak hatalar başvurusu</span><span class="sxs-lookup"><span data-stu-id="5b63c-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="5b63c-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b63c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5b63c-105">Bu konuda, Azure Apps hizmetinde ve IIS 'de ASP.NET Core uygulamaları barındırırken sık karşılaşılan hatalara yönelik sorun giderme önerileri sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5b63c-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="5b63c-106">Aşağıdaki bilgileri toplayın:</span><span class="sxs-lookup"><span data-stu-id="5b63c-106">Collect the following information:</span></span>

* <span data-ttu-id="5b63c-107">Tarayıcı davranışı (durum kodu ve hata iletisi)</span><span class="sxs-lookup"><span data-stu-id="5b63c-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="5b63c-108">Uygulama olay günlüğü girdileri</span><span class="sxs-lookup"><span data-stu-id="5b63c-108">Application Event Log entries</span></span>
  * <span data-ttu-id="5b63c-109">Azure App Service &ndash; bkz<xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="5b63c-109">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="5b63c-110">IIS</span><span class="sxs-lookup"><span data-stu-id="5b63c-110">IIS</span></span>
    1. <span data-ttu-id="5b63c-111">**Windows** menüsünde **Başlat** ' ı seçin, *Olay Görüntüleyicisi*yazın ve **ENTER**tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="5b63c-112">**Olay Görüntüleyicisi** açıldıktan sonra, kenar çubuğunda **Windows günlükleri** > **uygulaması** ' nı genişletin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="5b63c-113">ASP.NET Core modülü stdout ve hata ayıklama günlüğü girdileri</span><span class="sxs-lookup"><span data-stu-id="5b63c-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="5b63c-114">Azure App Service &ndash; bkz<xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="5b63c-114">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="5b63c-115">IIS &ndash; , ASP.NET Core modülü konusunun [günlük oluşturma ve yeniden yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) ve [Gelişmiş tanılama günlükleri](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) bölümlerindeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="5b63c-116">Hata bilgilerini aşağıdaki yaygın hatalarla karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="5b63c-117">Bir eşleşme bulunursa, sorun giderme talimatını izleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="5b63c-118">Bu konudaki hataların listesi ayrıntılı değildir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="5b63c-119">Burada listelenmeyen bir hatayla karşılaşırsanız, bu konunun en altındaki **içerik geri bildirim** düğmesini kullanarak yeni bir sorun açın ve hatayı yeniden oluşturma hakkında ayrıntılı yönergeler kullanın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="5b63c-120">Yükleyici VC + + yeniden dağıtılabilir alamıyor</span><span class="sxs-lookup"><span data-stu-id="5b63c-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="5b63c-121">**Yükleyici özel durumu:** 0x80072EFD **--veya--** 0x80072F76-belirtilmeyen hata</span><span class="sxs-lookup"><span data-stu-id="5b63c-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="5b63c-122">**Yükleyici günlüğü özel&#8224;durumu:** Hata 0x80072EFD **--veya--** 0x80072F76: EXE paketi yürütülemedi</span><span class="sxs-lookup"><span data-stu-id="5b63c-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="5b63c-123">&#8224;Günlük *C:\Users\{Kullanıcı} \AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="5b63c-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="5b63c-124">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-124">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-125">[.NET Core barındırma paketini yüklerken](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)sistemin Internet erişimi yoksa, bu özel durum yükleyicinin  *C++ Microsoft Visual 2015 yeniden dağıtılabilir*sürümünü almasını engellerse oluşur.</span><span class="sxs-lookup"><span data-stu-id="5b63c-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="5b63c-126">[Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=53840)' nden bir yükleyici edinin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="5b63c-127">Yükleyici başarısız olursa, sunucu [çerçeveye bağımlı bir dağıtımı (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd)barındırmak için gereken .NET Core çalışma zamanını alamayabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="5b63c-128">FDD barındırıyorsanız, çalışma zamanının **programlar & Özellikler** veya **uygulamalar & Özellikler**' de yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="5b63c-129">Belirli bir çalışma zamanı gerekliyse, [.net download arşivleri](https://dotnet.microsoft.com/download/archives) ' nden çalışma zamanını indirin ve sisteme yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="5b63c-130">Çalışma zamanını yükledikten sonra, bir komut isteminden net **stop was/y** ve ardından **net start w3svc** ' i yürüterek SISTEMI yeniden başlatın veya IIS 'yi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="5b63c-131">İşletim sistemi yükseltmesi 32-bit ASP.NET Core modülünü kaldırdı</span><span class="sxs-lookup"><span data-stu-id="5b63c-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="5b63c-132">**Uygulama günlüğü:** **C:\windows\system32\inetsrv\aspnetcore.dll** modül dll 'si yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="5b63c-133">Veriler hatadır.</span><span class="sxs-lookup"><span data-stu-id="5b63c-133">The data is the error.</span></span>

<span data-ttu-id="5b63c-134">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-134">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-135">Bir işletim sistemi yükseltmesi sırasında **C:\Windows\SysWOW64\inetsrv** dizininde işletim sistemi olmayan dosyalar korunmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="5b63c-136">ASP.NET Core modülü bir işletim sistemi yükseltmesinden önce yüklendiyse ve sonra herhangi bir uygulama havuzu bir işletim sistemi yükseltmesinden sonra 32 bit modda çalıştıktan sonra bu sorunla karşılaşılmıştır.</span><span class="sxs-lookup"><span data-stu-id="5b63c-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="5b63c-137">Bir işletim sistemi yükseltmesinden sonra ASP.NET Core modülünü onarın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="5b63c-138">Bkz. [.NET Core barındırma paketi 'Ni yüklemeyi](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="5b63c-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="5b63c-139">Yükleyici çalıştırıldığında **Onar** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-139">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="5b63c-140">Eksik site uzantısı, 32-bit (x86) ve 64-bit (x64) site uzantıları yüklü veya yanlış işlem bit genişliği ayarlanmış</span><span class="sxs-lookup"><span data-stu-id="5b63c-140">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="5b63c-141">*Azure Uygulama Hizmetleri tarafından barındırılan uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="5b63c-141">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="5b63c-142">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-142">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="5b63c-143">**Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="5b63c-143">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="5b63c-144">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-144">Could not find inprocess request handler.</span></span> <span data-ttu-id="5b63c-145">Hostfxr çağırmadan yakalanan çıkış: Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-145">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="5b63c-146">Belirtilen ' Microsoft. aspnetcore. App ' çerçevesi, ' {Version}-Preview-\*' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-146">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="5b63c-147">'/LM/W3SVC/1416782824/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.</span><span class="sxs-lookup"><span data-stu-id="5b63c-147">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="5b63c-148">**ASP.NET Core modülü stdout günlüğü:** Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-148">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="5b63c-149">Belirtilen ' Microsoft. aspnetcore. App ' çerçevesi, ' {Version}-Preview-\*' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-149">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-150">**ASP.NET Core modülü hata ayıklama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="5b63c-150">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="5b63c-151">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-151">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="5b63c-152">Başarısız HRESULT döndürüldü: 0x8000FFFF.</span><span class="sxs-lookup"><span data-stu-id="5b63c-152">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="5b63c-153">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-153">Could not find inprocess request handler.</span></span> <span data-ttu-id="5b63c-154">Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-154">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="5b63c-155">Belirtilen ' Microsoft. aspnetcore. App ' çerçevesi, ' {Version}-Preview-\*' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-155">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-156">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-156">Troubleshooting:</span></span>

* <span data-ttu-id="5b63c-157">Uygulamayı bir önizleme çalışma zamanı üzerinde çalıştırıyorsanız, uygulamanın ve uygulamanın çalışma zamanının bit durumuyla eşleşen 32-bit (x86) **veya** 64 bit (x64) site uzantısını da yükler.</span><span class="sxs-lookup"><span data-stu-id="5b63c-157">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="5b63c-158">**Uzantı veya birden çok çalışma zamanı sürümünü yüklemeyin.**</span><span class="sxs-lookup"><span data-stu-id="5b63c-158">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="5b63c-159">ASP.NET Core {RUNTIME VERSION} (x86) çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="5b63c-159">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="5b63c-160">ASP.NET Core {RUNTIME VERSION} (x64) çalışma zamanı</span><span class="sxs-lookup"><span data-stu-id="5b63c-160">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="5b63c-161">Uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-161">Restart the app.</span></span> <span data-ttu-id="5b63c-162">Uygulamanın yeniden başlatılması için birkaç saniye bekleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-162">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="5b63c-163">Uygulamayı bir önizleme çalışma zamanında çalıştırmak ve 32-bit (x86) ve 64 bit (x64) [site uzantıları](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) yüklüyse, uygulamanın bit durumuyla eşleşmeyen site uzantısını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-163">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="5b63c-164">Site uzantısını kaldırdıktan sonra uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-164">After removing the site extension, restart the app.</span></span> <span data-ttu-id="5b63c-165">Uygulamanın yeniden başlatılması için birkaç saniye bekleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-165">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="5b63c-166">Uygulamayı bir önizleme çalışma zamanında çalıştırmak ve site uzantısının bit kullanımı uygulamayla eşleşiyorsa, önizleme sitesi uzantısının *çalışma zamanı sürümünün* uygulamanın çalışma zamanı sürümüyle eşleştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-166">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="5b63c-167">**Uygulamanın uygulama ayarlarındaki** **platformunun** uygulamanın bit durumuyla eşleştiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="5b63c-167">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="5b63c-168">Daha fazla bilgi için bkz. <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="5b63c-168">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="5b63c-169">X86 uygulaması dağıtıldı, ancak uygulama havuzu 32-bit uygulamalar için etkinleştirilmemiş</span><span class="sxs-lookup"><span data-stu-id="5b63c-169">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="5b63c-170">**Tarayıcı:** HTTP hatası 500,30-Işlem Içi Işlem başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-170">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="5b63c-171">**Uygulama günlüğü:** ' {PATH} ' fiziksel köküne sahip '/LM/W3SVC/5/ROOT ' uygulaması beklenmeyen yönetilen özel duruma ulaştı, özel durum kodu = ' 0xe0434352 '.</span><span class="sxs-lookup"><span data-stu-id="5b63c-171">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="5b63c-172">Daha fazla bilgi için lütfen stderr günlüklerine bakın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-172">Please check the stderr logs for more information.</span></span> <span data-ttu-id="5b63c-173">' {PATH} ' fiziksel köküne sahip '/LM/W3SVC/5/ROOT ' uygulaması clr ve yönetilen uygulamayı yükleyemedi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-173">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="5b63c-174">CLR Worker iş parçacığından erken çıkıldı</span><span class="sxs-lookup"><span data-stu-id="5b63c-174">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="5b63c-175">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.</span><span class="sxs-lookup"><span data-stu-id="5b63c-175">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-176">**ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürüldü: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="5b63c-176">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="5b63c-177">Bu senaryo, kendi içinde bulunan bir uygulama yayımlanırken SDK tarafından yakalanarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="5b63c-177">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="5b63c-178">RID platform hedefi ile eşleşmezse SDK bir hata üretir (örneğin, `win10-x64` proje dosyasında ile `<PlatformTarget>x86</PlatformTarget>` RID).</span><span class="sxs-lookup"><span data-stu-id="5b63c-178">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="5b63c-179">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-179">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-180">X86 çerçevesine bağımlı bir dağıtım (`<PlatformTarget>x86</PlatformTarget>`) için, 32 bitlik uygulamalar için IIS uygulama havuzunu etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-180">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="5b63c-181">IIS Yöneticisi 'nde, uygulama havuzunun **Gelişmiş ayarlarını** açın ve **32 bitlik uygulamaları** **doğru**olarak etkinleştir ayarını yapın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-181">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="5b63c-182">Platform RID ile çakışıyor</span><span class="sxs-lookup"><span data-stu-id="5b63c-182">Platform conflicts with RID</span></span>

* <span data-ttu-id="5b63c-183">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-183">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="5b63c-184">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "C:\{Path} {Assembly} komut satırı ile işlem başlatamadı. { exe | dll} "', ErrorCode = ' 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="5b63c-184">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="5b63c-185">**ASP.NET Core modülü stdout günlüğü:** İşlenmeyen özel durum: System. BadImageFormatException: ' {ASSEMBLY}. dll ' dosyası veya bütünleştirilmiş kodu yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-185">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="5b63c-186">Bir programı hatalı biçimde yükleme girişiminde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="5b63c-186">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="5b63c-187">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-187">Troubleshooting:</span></span>

* <span data-ttu-id="5b63c-188">Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-188">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="5b63c-189">İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-189">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="5b63c-190">Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="5b63c-190">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="5b63c-191">Bu özel durum, bir uygulamayı yükseltirken ve daha yeni derlemeler dağıtıldığında bir Azure Apps dağıtımı için oluşursa, önceki dağıtımdan tüm dosyaları el ile silin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-191">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="5b63c-192">Yükseltilmiş bir uygulama dağıtımında, kalan uyumsuz `System.BadImageFormatException` derlemeler bir özel durumla sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-192">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="5b63c-193">URI uç noktası yanlış veya durdurulmuş Web sitesi</span><span class="sxs-lookup"><span data-stu-id="5b63c-193">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="5b63c-194">**Tarayıcı:** ERR_CONNECTION_REFUSED **--veya--** bağlantı kurulamıyor</span><span class="sxs-lookup"><span data-stu-id="5b63c-194">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="5b63c-195">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="5b63c-195">**Application Log:** No entry</span></span>

* <span data-ttu-id="5b63c-196">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-196">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-197">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-197">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-198">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-198">Troubleshooting:</span></span>

* <span data-ttu-id="5b63c-199">Uygulamanın kullanımda olduğu doğru URI uç noktasını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-199">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="5b63c-200">Bağlamaları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-200">Check the bindings.</span></span>

* <span data-ttu-id="5b63c-201">IIS Web sitesinin *durdurulmuş* durumda olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-201">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="5b63c-202">CoreWebEngine veya W3SVC sunucu özellikleri devre dışı</span><span class="sxs-lookup"><span data-stu-id="5b63c-202">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="5b63c-203">**İşletim sistemi özel durumu:** ASP.NET Core modülünü kullanmak için IIS 7,0 CoreWebEngine ve W3SVC özelliklerinin yüklü olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-203">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="5b63c-204">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-204">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-205">Uygun rol ve özelliklerin etkinleştirildiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-205">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="5b63c-206">Bkz. [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="5b63c-206">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="5b63c-207">Yanlış web sitesi fiziksel yolu veya uygulaması eksik</span><span class="sxs-lookup"><span data-stu-id="5b63c-207">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="5b63c-208">**Tarayıcı:** 403 Yasak-erişim reddedildi **--veya--** 403,14 yasak-Web sunucusu bu dizinin içeriğini listebir şekilde yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="5b63c-208">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="5b63c-209">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="5b63c-209">**Application Log:** No entry</span></span>

* <span data-ttu-id="5b63c-210">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-210">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-211">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-211">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-212">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-212">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-213">IIS Web sitesi **temel ayarları** ve fiziksel uygulama klasörü ' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-213">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="5b63c-214">Uygulamanın IIS Web sitesi **fiziksel yolundaki**klasörde olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-214">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="5b63c-215">Yanlış rol, ASP.NET Core modülü yüklü değil veya yanlış izinler</span><span class="sxs-lookup"><span data-stu-id="5b63c-215">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="5b63c-216">**Tarayıcı:** 500,19 iç sunucu hatası-sayfa için ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor.</span><span class="sxs-lookup"><span data-stu-id="5b63c-216">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="5b63c-217">**--Veya--** Bu sayfa görüntülenemiyor</span><span class="sxs-lookup"><span data-stu-id="5b63c-217">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="5b63c-218">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="5b63c-218">**Application Log:** No entry</span></span>

* <span data-ttu-id="5b63c-219">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-219">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-220">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-220">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-221">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-221">Troubleshooting:</span></span>

* <span data-ttu-id="5b63c-222">Doğru rolün etkin olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-222">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="5b63c-223">Bkz. [IIS yapılandırması](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="5b63c-223">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="5b63c-224">**Programlar & Özellikler** veya **uygulamalar & özellikleri** açın ve **Windows Server barındırma** 'nın yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-224">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="5b63c-225">Yüklü programlar listesinde **Windows Server barındırma** yoksa, .NET Core barındırma paketi ' ni indirip yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-225">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="5b63c-226">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="5b63c-226">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="5b63c-227">Daha fazla bilgi için bkz. [.NET Core barındırma paketini yüklemeye](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="5b63c-227">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="5b63c-228">**Uygulama havuzu** > **işlem modeli** > **kimliğinin** **applicationpokaydentity** olarak ayarlandığından veya özel kimliğin uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="5b63c-228">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="5b63c-229">ASP.NET Core barındırma paketini kaldırdıysanız ve barındırma paketinin önceki bir sürümünü yüklediyseniz *ApplicationHost. config* dosyası ASP.NET Core modülü için bir bölüm içermez.</span><span class="sxs-lookup"><span data-stu-id="5b63c-229">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="5b63c-230">*ApplicationHost. config* dosyasını *% windir%/system32/inetsrv/config* konumunda açın `<configuration><configSections><sectionGroup name="system.webServer">` ve bölüm grubunu bulun.</span><span class="sxs-lookup"><span data-stu-id="5b63c-230">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="5b63c-231">Bölüm grubunda ASP.NET Core modülünün bölümü eksikse, Bölüm öğesini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5b63c-231">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="5b63c-232">Alternatif olarak, ASP.NET Core barındıran paketin en son sürümünü de yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-232">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="5b63c-233">En son sürüm, desteklenen ASP.NET Core uygulamalarla geriye dönük olarak uyumludur.</span><span class="sxs-lookup"><span data-stu-id="5b63c-233">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="5b63c-234">Hatalı processPath, eksik yol değişkeni, barındırma paketi yüklü değil, sistem/IIS yeniden başlatılmadı, VC + + yeniden dağıtılabilir yüklü değil veya DotNet. exe erişim ihlali</span><span class="sxs-lookup"><span data-stu-id="5b63c-234">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-235">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-235">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="5b63c-236">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "{...}" komut satırı ile işleme başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="5b63c-236">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="5b63c-237">', ErrorCode = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="5b63c-237">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="5b63c-238">' {PATH} ' uygulaması başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-238">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="5b63c-239">' {PATH} ' konumunda yürütülebilir dosya bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-239">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="5b63c-240">'/LM/W3SVC/2/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8007023e '.</span><span class="sxs-lookup"><span data-stu-id="5b63c-240">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="5b63c-241">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-241">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="5b63c-242">**ASP.NET Core modülü hata ayıklama günlüğü:** Olay günlüğü: ' {PATH} ' uygulaması başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-242">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="5b63c-243">' {PATH} ' konumunda yürütülebilir dosya bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-243">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="5b63c-244">Başarısız HRESULT döndürüldü: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="5b63c-244">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="5b63c-245">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-245">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="5b63c-246">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "{...}" komut satırı ile işleme başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="5b63c-246">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="5b63c-247">', ErrorCode = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="5b63c-247">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="5b63c-248">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.</span><span class="sxs-lookup"><span data-stu-id="5b63c-248">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-249">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-249">Troubleshooting:</span></span>

* <span data-ttu-id="5b63c-250">Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-250">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="5b63c-251">İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-251">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="5b63c-252">Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="5b63c-252">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="5b63c-253">Bir çerçeveye bağımlı dağıtım (FDD `<aspNetCore>` ) veya `.\{ASSEMBLY}.exe` [kendi kendine ait dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)için `dotnet` olduğunu doğrulamak üzere *Web. config* dosyasındaki öğesindeki *processPath* özniteliğini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-253">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="5b63c-254">FDD için, *DotNet. exe* ' nin yol ayarları aracılığıyla erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-254">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="5b63c-255">*C:\Program files\dotnet\\*  dosyasının sistem yolu ayarlarında bulunduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-255">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="5b63c-256">FDD için, *DotNet. exe* ' yi uygulama havuzunun Kullanıcı kimliği için erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-256">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="5b63c-257">Uygulama havuzu Kullanıcı kimliğinin *C:\Program Files\dotnet* dizinine erişimi olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-257">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="5b63c-258">*C:\Program Files\dotnet* ve uygulama dizinlerindeki uygulama havuzu Kullanıcı kimliği için yapılandırılmış reddetme kuralı olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-258">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="5b63c-259">Bir FDD dağıtılmış ve IIS 'nin yeniden başlatılmasına gerek kalmadan .NET Core yüklenmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-259">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="5b63c-260">Bir komut isteminden net **stop was/y** ve ardından **net start w3svc** ' i yürüterek sunucuyu YENIDEN başlatın ya da IIS 'yi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-260">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="5b63c-261">Bir FDD, barındırma sistemine .NET Core çalışma zamanı yüklenmeden dağıtılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-261">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="5b63c-262">.NET Core çalışma zamanı yüklenmemişse, sistemde **.NET Core barındırma paketi yükleyicisini** çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-262">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="5b63c-263">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="5b63c-263">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="5b63c-264">Daha fazla bilgi için bkz. [.NET Core barındırma paketini yüklemeye](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="5b63c-264">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="5b63c-265">Belirli bir çalışma zamanı gerekliyse, [.net download arşivleri](https://dotnet.microsoft.com/download/archives) ' nden çalışma zamanını indirin ve sisteme yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-265">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="5b63c-266">Bir komut isteminden net **stop idi** ve ardından **net start w3svc** ' i yürüterek SISTEMI yeniden başlatarak veya IIS 'yi yeniden başlatarak yüklemeyi doldurun.</span><span class="sxs-lookup"><span data-stu-id="5b63c-266">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="5b63c-267">Bir FDD dağıtılmış olabilir ve sistemde *Microsoft Visual C++ 2015 Redistributable (x64)* yüklü değildir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-267">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="5b63c-268">[Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=53840)' nden bir yükleyici edinin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-268">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="5b63c-269">\<Aspnetcore > öğesinin bağımsız değişkenleri yanlış</span><span class="sxs-lookup"><span data-stu-id="5b63c-269">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-270">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-270">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="5b63c-271">**Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="5b63c-271">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="5b63c-272">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-272">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="5b63c-273">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-273">Could not find inprocess request handler.</span></span> <span data-ttu-id="5b63c-274">Hostfxr çağırmadan yakalanan çıkış: DotNet SDK komutlarını çalıştırmak mı istediniz?</span><span class="sxs-lookup"><span data-stu-id="5b63c-274">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="5b63c-275">Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 '/LM/W3SVC/3/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.</span><span class="sxs-lookup"><span data-stu-id="5b63c-275">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="5b63c-276">**ASP.NET Core modülü stdout günlüğü:** DotNet SDK komutlarını çalıştırmak mı istediniz?</span><span class="sxs-lookup"><span data-stu-id="5b63c-276">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="5b63c-277">Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="5b63c-277">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="5b63c-278">**ASP.NET Core modülü hata ayıklama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="5b63c-278">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="5b63c-279">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-279">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="5b63c-280">Başarısız HRESULT döndürüldü: 0x8000FFFF, InProcess istek işleyicisi bulamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-280">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="5b63c-281">Hostfxr çağırmadan yakalanan çıkış: DotNet SDK komutlarını çalıştırmak mı istediniz?</span><span class="sxs-lookup"><span data-stu-id="5b63c-281">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="5b63c-282">Lütfen şu kaynaktan DotNet SDK 'Yı yüklemelisiniz: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Başarısız HRESULT döndürüldü: 0x8000FFFF</span><span class="sxs-lookup"><span data-stu-id="5b63c-282">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="5b63c-283">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-283">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="5b63c-284">**Uygulama günlüğü:** ' C:\{Path}\' fiziksel köküne sahip ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, ' "DotNet" CommandLine ile işlemi başlatamadı.\{ ASSEMBLY}. dll ', ErrorCode = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="5b63c-284">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="5b63c-285">**ASP.NET Core modülü stdout günlüğü:** Yürütülecek uygulama yok: ' Yol\{derlemesi}. dll '</span><span class="sxs-lookup"><span data-stu-id="5b63c-285">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="5b63c-286">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-286">Troubleshooting:</span></span>

* <span data-ttu-id="5b63c-287">Uygulamanın Kestrel üzerinde yerel olarak çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-287">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="5b63c-288">İşlem hatası, uygulamanın içindeki bir sorunun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-288">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="5b63c-289">Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="5b63c-289">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="5b63c-290">Bir çerçeveye bağımlı dağıtım (FDD) veya (b) yok, boş bir dize`arguments=""`() veya `.\{ASSEMBLY}.dll` bir listesi olduğunu doğrulamak için *Web. config* dosyasındaki `<aspNetCore>` öğesindeki *arguments* özniteliğini inceleyin. kendi içinde bir dağıtım`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`(SCD) için uygulamanın bağımsız değişkenleri ().</span><span class="sxs-lookup"><span data-stu-id="5b63c-290">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="5b63c-291">Eksik .NET Core paylaşılan çerçevesi</span><span class="sxs-lookup"><span data-stu-id="5b63c-291">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="5b63c-292">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-292">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="5b63c-293">**Uygulama günlüğü:** InProcess istek işleyicisini bulmak için hostfxr çağırma hiçbir yerel bağımlılığı bulamamadan başarısız oldu.</span><span class="sxs-lookup"><span data-stu-id="5b63c-293">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="5b63c-294">Büyük olasılıkla uygulamanın yanlış yapılandırılmış olduğu anlamına gelir, lütfen uygulamanın hedeflediği ve makinede yüklü olduğu Microsoft. NetCore. App ve Microsoft. AspNetCore. app sürümlerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5b63c-294">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="5b63c-295">InProcess istek işleyicisi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-295">Could not find inprocess request handler.</span></span> <span data-ttu-id="5b63c-296">Hostfxr çağırmadan yakalanan çıkış: Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-296">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="5b63c-297">Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION} ' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-297">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="5b63c-298">'/LM/W3SVC/5/ROOT ' uygulaması başlatılamadı, hata kodu ' 0x8000FFFF '.</span><span class="sxs-lookup"><span data-stu-id="5b63c-298">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="5b63c-299">**ASP.NET Core modülü stdout günlüğü:** Uyumlu bir çerçeve sürümü bulmak mümkün değildi.</span><span class="sxs-lookup"><span data-stu-id="5b63c-299">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="5b63c-300">Belirtilen ' Microsoft. AspNetCore. App ' çerçevesi, ' {VERSION} ' sürümü bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-300">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="5b63c-301">**ASP.NET Core modülü hata ayıklama günlüğü:** Başarısız HRESULT döndürüldü: 0x8000FFFF</span><span class="sxs-lookup"><span data-stu-id="5b63c-301">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="5b63c-302">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-302">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-303">Çerçeveye bağımlı bir dağıtım (FDD) için, sistemde doğru çalışma zamanının yüklü olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-303">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="5b63c-304">Uygulama havuzu durduruldu</span><span class="sxs-lookup"><span data-stu-id="5b63c-304">Stopped Application Pool</span></span>

* <span data-ttu-id="5b63c-305">**Tarayıcı:** 503 hizmeti kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="5b63c-305">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="5b63c-306">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="5b63c-306">**Application Log:** No entry</span></span>

* <span data-ttu-id="5b63c-307">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-307">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-308">**ASP.NET Core modülü hata ayıklama günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-308">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-309">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-309">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-310">Uygulama havuzunun *durdurulmuş* durumda olmadığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="5b63c-310">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="5b63c-311">Alt uygulama bir \<işleyiciler > bölümü içerir</span><span class="sxs-lookup"><span data-stu-id="5b63c-311">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="5b63c-312">**Tarayıcı:** HTTP hatası 500,19-Iç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-312">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="5b63c-313">**Uygulama günlüğü:** Giriş yok</span><span class="sxs-lookup"><span data-stu-id="5b63c-313">**Application Log:** No entry</span></span>

* <span data-ttu-id="5b63c-314">**ASP.NET Core modülü stdout günlüğü:** Kök uygulamanın günlük dosyası oluşturulur ve normal işlemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-314">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="5b63c-315">Alt uygulamanın günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-315">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-316">**ASP.NET Core modülü hata ayıklama günlüğü:** Kök uygulamanın günlük dosyası oluşturulur ve normal işlemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-316">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="5b63c-317">Alt uygulamanın günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-317">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-318">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-318">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5b63c-319">Alt uygulamanın *Web. config* dosyasının bir `<handlers>` bölüm içermediğinden veya alt uygulamanın üst uygulamanın işleyicilerini almadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="5b63c-319">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="5b63c-320">*Web. config* dosyasının `<system.webServer>` üst uygulamanın bölümü bir `<location>` öğesinin içine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-320">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="5b63c-321">Özelliği, [Konum \<>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi `false` içinde belirtilen ayarların üst uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını belirtmek için olarak ayarlanır. <xref:System.Configuration.SectionInformation.InheritInChildApplications*></span><span class="sxs-lookup"><span data-stu-id="5b63c-321">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="5b63c-322">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="5b63c-322">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5b63c-323">Alt uygulamanın *Web. config* dosyasının bir `<handlers>` bölüm içermediğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="5b63c-323">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="5b63c-324">stdout günlük yolu yanlış</span><span class="sxs-lookup"><span data-stu-id="5b63c-324">stdout log path incorrect</span></span>

* <span data-ttu-id="5b63c-325">**Tarayıcı:** Uygulama normal olarak yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="5b63c-325">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-326">**Uygulama günlüğü:** C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-326">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="5b63c-327">Özel durum iletisi: {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84 konumunda HRESULT 0x80070005 döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="5b63c-327">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="5b63c-328">C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi durdurulamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-328">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="5b63c-329">Özel durum iletisi: HRESULT 0x80070002, {PATH} konumunda döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="5b63c-329">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="5b63c-330">{PATH} \aspnetcorev2_ınprocess.exe içinde stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-330">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="5b63c-331">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-331">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="5b63c-332">**ASP.NET Core modülü hata ayıklama günlüğü:** C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-332">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="5b63c-333">Özel durum iletisi: {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84 konumunda HRESULT 0x80070005 döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="5b63c-333">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="5b63c-334">C:\Program Files\IIS\Asp.Net Core Module\v2\aspnetcorev2.dll' de stdout yeniden yönlendirmesi durdurulamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-334">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="5b63c-335">Özel durum iletisi: HRESULT 0x80070002, {PATH} konumunda döndürüldü.</span><span class="sxs-lookup"><span data-stu-id="5b63c-335">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="5b63c-336">{PATH} \aspnetcorev2_ınprocess.exe içinde stdout yeniden yönlendirmesi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-336">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="5b63c-337">**Uygulama günlüğü:** Uyarı: StdoutLogFile \\oluşturulamadı mi?\{ YOL} \path_doesnt_exist\stdout_{PROCESS ID} _ {TIMESTAMP}. log, ErrorCode =-2147024893.</span><span class="sxs-lookup"><span data-stu-id="5b63c-337">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="5b63c-338">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b63c-338">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-339">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-339">Troubleshooting:</span></span>

* <span data-ttu-id="5b63c-340">*Web.* config `<aspNetCore>` öğesinin öğesinde belirtilen yolyok.`stdoutLogFile`</span><span class="sxs-lookup"><span data-stu-id="5b63c-340">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="5b63c-341">Daha fazla bilgi için bkz [. asp.NET Core modülü: Günlük oluşturma ve yeniden](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="5b63c-341">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="5b63c-342">Uygulama havuzu kullanıcısının stdout günlük yoluna yazma erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="5b63c-342">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="5b63c-343">Uygulama yapılandırması genel sorunu</span><span class="sxs-lookup"><span data-stu-id="5b63c-343">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5b63c-344">**Tarayıcı:** HTTP hatası 500,0-Işlem Içi Işleyici yükleme hatası **--veya--** HTTP hatası 500,30-Ancm Işlem Içi başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-344">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="5b63c-345">**Uygulama günlüğü:** Değişken</span><span class="sxs-lookup"><span data-stu-id="5b63c-345">**Application Log:** Variable</span></span>

* <span data-ttu-id="5b63c-346">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boş veya, uygulamanın noktası başarısız olana kadar normal girdilerle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5b63c-346">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="5b63c-347">**ASP.NET Core modülü hata ayıklama günlüğü:** Değişken</span><span class="sxs-lookup"><span data-stu-id="5b63c-347">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="5b63c-348">**Tarayıcı:** HTTP hatası 502,5-Işlem hatası</span><span class="sxs-lookup"><span data-stu-id="5b63c-348">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="5b63c-349">**Uygulama günlüğü:** Fiziksel kökü\{' c: Path}\' olan ' MACHINE/Webroot/apphost/{Assembly} ' uygulaması, CommandLine ' "c:\{Path}\{Assembly} ile oluşturulmuş işlem. { exe | dll} "', ancak belirtilen ' {PORT} ' bağlantı noktasında kilitlendi veya yanıt vermedi ya da bu bağlantı noktası üzerinde dinleme yapamadı, ErrorCode = ' {ERROR CODE} '</span><span class="sxs-lookup"><span data-stu-id="5b63c-349">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="5b63c-350">**ASP.NET Core modülü stdout günlüğü:** Günlük dosyası oluşturulur ancak boştur.</span><span class="sxs-lookup"><span data-stu-id="5b63c-350">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="5b63c-351">Sorun Giderme:</span><span class="sxs-lookup"><span data-stu-id="5b63c-351">Troubleshooting:</span></span>

<span data-ttu-id="5b63c-352">Büyük olasılıkla uygulama yapılandırması veya programlama sorunu nedeniyle işlem başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="5b63c-352">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="5b63c-353">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="5b63c-353">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
