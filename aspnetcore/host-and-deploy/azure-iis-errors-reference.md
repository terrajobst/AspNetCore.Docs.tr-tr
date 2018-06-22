---
title: Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu
author: guardrex
description: Sık karşılaşılan Azure uygulama hizmeti ve IIS üzerinde ASP.NET Core uygulamaları barındırdığında ayırt.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 30b7f1d8e1cfdfd3d1db865ff428eb2094a84d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277325"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="bdbdb-103">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="bdbdb-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="bdbdb-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bdbdb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bdbdb-105">Aşağıdaki hatalar, tam bir listesi değildir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="bdbdb-106">Burada listelenmeyen bir hatayla karşılaşırsanız [yeni bir sorun açmak](https://github.com/aspnet/Docs/issues/new) hatayı yeniden oluşturmaya yönelik ayrıntılı yönergeler ile.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="bdbdb-107">Aşağıdaki bilgileri toplayın:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-107">Collect the following information:</span></span>

* <span data-ttu-id="bdbdb-108">Tarayıcı davranışı</span><span class="sxs-lookup"><span data-stu-id="bdbdb-108">Browser behavior</span></span>
* <span data-ttu-id="bdbdb-109">Uygulama olay günlüğü girişleri</span><span class="sxs-lookup"><span data-stu-id="bdbdb-109">Application Event Log entries</span></span>
* <span data-ttu-id="bdbdb-110">ASP.NET Core modül stdout günlük girişleri</span><span class="sxs-lookup"><span data-stu-id="bdbdb-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="bdbdb-111">Aşağıdaki sık karşılaşılan bilgileri karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="bdbdb-112">Bir eşleşme olursa, sorun giderme önerileri izleyin.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="bdbdb-113">Yükleyici VC ++ Redistributable alınamıyor</span><span class="sxs-lookup"><span data-stu-id="bdbdb-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="bdbdb-114">**Yükleyici özel durum:** 0x80072efd veya 0x80072f76 - belirtilmeyen hata</span><span class="sxs-lookup"><span data-stu-id="bdbdb-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="bdbdb-115">**Yükleyici günlük özel durum&#8224;:** hata 0x80072efd veya 0x80072f76: EXE paketini yürütülemedi.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="bdbdb-116">&#8224;Günlük C:\Users bulunduğu\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="bdbdb-117">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-117">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-118">Sistem, barındırma paket yüklenirken Internet erişimi yoksa, bu özel durum oluştu yükleyici almasını engellendiğinde *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-118">If the system doesn't have Internet access while installing the Hosting Bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="bdbdb-119">Bir yükleyicisinden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="bdbdb-120">Yükleyici başarısız olursa sunucunun framework bağımlı dağıtım (FDD) barındırmak için gerekli .NET çekirdeği çalışma zamanı almayabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="bdbdb-121">Bir FDD barındırma, çalışma zamanı programlarında yüklendiğinden emin olmak &amp; özellikleri.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="bdbdb-122">Gerekirse, bir çalışma zamanı Yükleyicisi'nden elde [.NET tüm yüklemelerini](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="bdbdb-123">Çalışma zamanı yüklendikten sonra sistemi yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="bdbdb-124">İşletim sistemi yükseltme 32-bit ASP.NET Core modül kaldırılır</span><span class="sxs-lookup"><span data-stu-id="bdbdb-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="bdbdb-125">**Uygulama günlüğü:** modül DLL'si **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="bdbdb-126">Veride hata yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-126">The data is the error.</span></span>

<span data-ttu-id="bdbdb-127">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-127">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-128">İşletim sistemi olmayan dosyaları **C:\Windows\SysWOW64\inetsrv** directory olmayan bir işletim sistemi sırasında korunur yükseltme.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="bdbdb-129">ASP.NET çekirdeği modülü öncesinde yüklüyse, bir işletim sistemi yükseltme ve ardından tüm AppPool çalıştırıldığında 32-bit modunda işletim sistemi yükseltme işleminden sonra bu sorunla karşılaştı.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="bdbdb-130">Bir işletim sistemi yükseltme sonrasında ASP.NET Core Modülü'nü onarın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="bdbdb-131">Bkz: [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-131">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="bdbdb-132">Seçin **onarım** yükleyici çalıştırıldığında.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="bdbdb-133">RID Platform çakışıyor</span><span class="sxs-lookup"><span data-stu-id="bdbdb-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="bdbdb-134">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bdbdb-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bdbdb-135">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' komut satırı ile işlemi başlatılamadı ' "C:\\{PATH} {derleme}. { exe | dll} "', hata kodu = ' 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="bdbdb-136">**ASP.NET çekirdeği modülü günlüğü:** işlenmeyen bir özel durum: System.BadImageFormatException: '{} derlemesi .dll' dosya veya derleme yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="bdbdb-137">Yanlış biçime sahip bir program yüklemek için girişimde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="bdbdb-138">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-138">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-139">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bdbdb-140">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bdbdb-141">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bdbdb-142">Onaylayın `<PlatformTarget>` içinde *.csproj* RID ile çakışan değil.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="bdbdb-143">Örneğin, belirtmeyin bir `<PlatformTarget>` , `x86` ve bir RID yayınlama `win10-x64`, kullanarak ya da *dotnet Yayımla - c yayın - r win10-x64* veya ayarlayarak `<RuntimeIdentifiers>` içinde *.csproj*  için `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="bdbdb-144">Proje uyarı veya hata yayımlar ancak sistemdeki yukarıdaki oturum durumlarla başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="bdbdb-145">Bu özel bir Azure uygulamaları dağıtım için bir uygulama yükseltirken oluşur ve yeni derlemeleri dağıtma el ile tüm dosyaların önceki dağıtımından silerseniz.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="bdbdb-146">Uyumsuz derlemeleri kalan içinde sonuçlanabilir bir `System.BadImageFormatException` yükseltilmiş bir uygulama dağıtımı sırasında özel durum.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="bdbdb-147">URI uç nokta yanlış ya da durdurulmuş Web sitesi</span><span class="sxs-lookup"><span data-stu-id="bdbdb-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="bdbdb-148">**Tarayıcı:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="bdbdb-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="bdbdb-149">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bdbdb-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="bdbdb-150">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bdbdb-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bdbdb-151">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-151">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-152">Doğru URI uç nokta uygulaması için kullanılan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="bdbdb-153">Bağlamaları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-153">Check the bindings.</span></span>

* <span data-ttu-id="bdbdb-154">IIS Web sitesinin içinde olmadığını onaylayın *durduruldu* durumu.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="bdbdb-155">Devre dışı CoreWebEngine veya W3SVC sunucusu özellikleri</span><span class="sxs-lookup"><span data-stu-id="bdbdb-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="bdbdb-156">**İşletim sistemi özel durum:** ASP.NET Core modülü kullanmak için IIS 7.0 CoreWebEngine ve W3SVC özellikleri yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="bdbdb-157">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-157">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-158">Uygun rol ve özellikleri etkinleştirildiğini onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="bdbdb-159">Bkz: [IIS yapılandırmasını](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="bdbdb-160">Yanlış Web sitesi fiziksel yolu veya uygulama eksik</span><span class="sxs-lookup"><span data-stu-id="bdbdb-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="bdbdb-161">**Tarayıcı:** 403 Yasak - erişim reddedildi **--veya--** 403.14 Yasak - Web sunucusu bu dizinin içindekileri listelemeyecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="bdbdb-162">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bdbdb-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="bdbdb-163">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bdbdb-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bdbdb-164">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-164">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-165">IIS Web sitesini denetleyin **temel ayarları** ve fiziksel uygulama klasör.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="bdbdb-166">Uygulama IIS Web sitesinde klasöründe olduğunu onaylayın **fiziksel yolu**.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="bdbdb-167">Yanlış rol, modül yüklü değil veya yanlış izinler</span><span class="sxs-lookup"><span data-stu-id="bdbdb-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="bdbdb-168">**Tarayıcı:** iç sunucu hatası 500.19 - sayfayla ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="bdbdb-169">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bdbdb-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="bdbdb-170">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bdbdb-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bdbdb-171">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-171">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-172">Uygun rolde etkinleştirilmiş olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="bdbdb-173">Bkz: [IIS yapılandırmasını](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="bdbdb-174">Denetleme **programlar &amp; özellikleri** onaylayın **Microsoft ASP.NET Core Modülü** yüklendi.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="bdbdb-175">Varsa **Microsoft ASP.NET Core Modülü** yüklü programlar listesinde yer almayan modülünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="bdbdb-176">Bkz: [paket barındırma .NET Core yükleme](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-176">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="bdbdb-177">Olduğundan emin olun **uygulama havuzu** > **işlem modeli** > **kimlik** ayarlanır **ApplicationPoolIdentity** veya özel kimlik uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="bdbdb-178">Yanlış processPath, eksik PATH değişkeni, yüklü paketi barındırma, sistem/IIS yeniden, VC ++ yüklü değil, Redistributable veya dotnet.exe erişim ihlali</span><span class="sxs-lookup"><span data-stu-id="bdbdb-178">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="bdbdb-179">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bdbdb-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bdbdb-180">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' ".\{ derleme} .exe"', hata kodu = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="bdbdb-181">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş</span><span class="sxs-lookup"><span data-stu-id="bdbdb-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="bdbdb-182">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-182">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-183">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bdbdb-184">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bdbdb-185">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bdbdb-186">Denetleme *processPath* özniteliği `<aspNetCore>` öğesinde *web.config* olduğundan emin olmak için *dotnet* framework bağımlı dağıtım (FDD) veya *. \{derleme} .exe* müstakil dağıtımı (SCD).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="bdbdb-187">Bir FDD için *dotnet.exe* yol ayarları erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="bdbdb-188">Onaylayın * C:\Program Files\dotnet\* sistem yolu ayarlarında bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="bdbdb-189">Bir FDD için *dotnet.exe* uygulama havuzu kullanıcı kimliği için erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="bdbdb-190">Uygulama havuzu kullanıcı kimliği için erişimi olduğunu doğrulamak *C:\Program Files\dotnet* dizini.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="bdbdb-191">Uygulama havuzu kullanıcı kimliği için yapılandırılmış hiçbir izin verme kuralı olduğundan emin olun *C:\Program Files\dotnet* ve uygulama dizinleri.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="bdbdb-192">Bir FDD dağıtılan ve IIS'yi yeniden başlatmadan .NET Core yüklü.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="bdbdb-193">Sunucuyu yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bdbdb-194">Bir FDD barındıran sistemde .NET çekirdeği çalışma zamanı yüklemeden dağıtılmış.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="bdbdb-195">.NET çekirdeği çalışma zamanı yüklenmemiştir çalıştırırsanız **.NET Core barındırma Paket Yükleyici** sistemdeki.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span> <span data-ttu-id="bdbdb-196">Bkz: [paket barındırma .NET Core yükleme](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-196">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="bdbdb-197">Internet bağlantısı olmadan bir sistemde .NET çekirdeği çalışma zamanı yükleme girişimi varsa, çalışma alanından elde [.NET tüm yüklemelerini](https://www.microsoft.com/net/download/all) ve ASP.NET Core modülünü yüklemek için barındırma paket yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the Hosting Bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="bdbdb-198">Sistemi yeniden başlatmayı veya yürüterek IIS yeniden yüklemeyi tamamlamak **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bdbdb-199">Bir FDD dağıtılan ve *Microsoft Visual C++ 2015 Redistributable (x64)* sistemde yüklü değil.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="bdbdb-200">Bir yükleyicisinden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="bdbdb-201">Yanlış bağımsız \<aspNetCore\> öğesi</span><span class="sxs-lookup"><span data-stu-id="bdbdb-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="bdbdb-202">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bdbdb-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bdbdb-203">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' "dotnet".\{ .dll derleme}', hata kodu = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="bdbdb-204">**ASP.NET çekirdeği modülü günlüğü:** yürütmek için uygulama yok: ' yolu\{derleme} .dll '</span><span class="sxs-lookup"><span data-stu-id="bdbdb-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="bdbdb-205">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-205">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-206">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bdbdb-207">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bdbdb-208">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bdbdb-209">İncelemek *bağımsız değişkenleri* özniteliği `<aspNetCore>` öğesinde *web.config* ya da olduğunu onaylamak için (a) *.\{ derleme} .dll* framework bağımlı dağıtım (FDD); veya (b yok, boş bir dize) (*bağımsız değişkenleri = ""*), veya bir uygulamanın bağımsız değişkenleri listesi (*bağımsız değişkenler "arg1, arg2,..." =*) kendi içinde bulunan dağıtımlar için (SCD).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="bdbdb-210">Eksik .NET Framework sürümü</span><span class="sxs-lookup"><span data-stu-id="bdbdb-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="bdbdb-211">**Tarayıcı:** 502.3 hatalı ağ geçidi - bağlantı hatası isteği yönlendirmek çalışılırken oluştu.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="bdbdb-212">**Uygulama günlüğü:** HataKodu = uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' "dotnet".\{ .dll derleme}', hata kodu = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="bdbdb-213">**ASP.NET çekirdeği modülü günlüğü:** yöntemi, dosya veya derleme özel durum eksik.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="bdbdb-214">Yöntemi, dosya veya derleme özel durum belirtildi bir .NET Framework yöntemi, dosya veya derleme olabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="bdbdb-215">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-215">Troubleshooting:</span></span>

* <span data-ttu-id="bdbdb-216">Sistemden eksik .NET Framework sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="bdbdb-217">Bir framework bağımlı dağıtım (FDD) için doğru çalışma zamanı sistemde yüklü olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="bdbdb-218">Proje 1.1 barındırma sisteme dağıtılan 2.0 sürümüne yükseltilir ve bu özel durumun sonucu, 2.0 framework barındıran sistemde olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="bdbdb-219">Durdurulan uygulama havuzunu</span><span class="sxs-lookup"><span data-stu-id="bdbdb-219">Stopped Application Pool</span></span>

* <span data-ttu-id="bdbdb-220">**Tarayıcı:** 503 Hizmet kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="bdbdb-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="bdbdb-221">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bdbdb-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="bdbdb-222">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bdbdb-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bdbdb-223">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bdbdb-223">Troubleshooting</span></span>

* <span data-ttu-id="bdbdb-224">Uygulama havuzu içinde olmadığını onaylayın *durduruldu* durumu.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="bdbdb-225">IIS tümleştirme Ara uygulanmadı</span><span class="sxs-lookup"><span data-stu-id="bdbdb-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="bdbdb-226">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bdbdb-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bdbdb-227">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH}\{derleme}. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =</span><span class="sxs-lookup"><span data-stu-id="bdbdb-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="bdbdb-228">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve normal işlemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="bdbdb-229">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bdbdb-229">Troubleshooting</span></span>

* <span data-ttu-id="bdbdb-230">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bdbdb-231">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bdbdb-232">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bdbdb-233">Ya da onaylayın:</span><span class="sxs-lookup"><span data-stu-id="bdbdb-233">Confirm that either:</span></span>
  * <span data-ttu-id="bdbdb-234">Referencedby IIS tümleştirme Ara yazılımıdır çağırma `UseIISIntegration` uygulamanın yöntemi `WebHostBuilder` (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="bdbdb-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="bdbdb-235">Uygulamaları kullanan `CreateDefaultBuilder` yöntemi (ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="bdbdb-236">Bkz: [ASP.NET Core ana](xref:fundamentals/host/index) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-236">See [Host in ASP.NET Core](xref:fundamentals/host/index) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="bdbdb-237">Alt uygulama içeren bir \<işleyicileri\> bölümü</span><span class="sxs-lookup"><span data-stu-id="bdbdb-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="bdbdb-238">**Tarayıcı:** HTTP Hatası 500.19 - iç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="bdbdb-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="bdbdb-239">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bdbdb-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="bdbdb-240">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve kök uygulaması için normal işlem gösterir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="bdbdb-241">Günlük dosyası için alt uygulaması oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="bdbdb-242">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bdbdb-242">Troubleshooting</span></span>

* <span data-ttu-id="bdbdb-243">Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="bdbdb-244">STDOUT günlük yolu yanlış</span><span class="sxs-lookup"><span data-stu-id="bdbdb-244">stdout log path incorrect</span></span>

* <span data-ttu-id="bdbdb-245">**Tarayıcı:** uygulama normalde yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="bdbdb-246">**Uygulama günlüğü:** Uyarı: stdoutLogFile oluşturulamadı \\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, HataKodu = - 2147024893.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="bdbdb-247">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bdbdb-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bdbdb-248">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bdbdb-248">Troubleshooting</span></span>

* <span data-ttu-id="bdbdb-249">`stdoutLogFile` Belirtilen yola `<aspNetCore>` öğesinin *web.config* yok.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="bdbdb-250">Daha fazla bilgi için bkz: [günlük oluşturma ve yeniden yönlendirme](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) ASP.NET Core modülü yapılandırması başvuru konusu bölümü.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="bdbdb-251">Uygulama yapılandırma genel sorunu</span><span class="sxs-lookup"><span data-stu-id="bdbdb-251">Application configuration general issue</span></span>

* <span data-ttu-id="bdbdb-252">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bdbdb-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bdbdb-253">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH}\{derleme}. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =</span><span class="sxs-lookup"><span data-stu-id="bdbdb-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="bdbdb-254">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş</span><span class="sxs-lookup"><span data-stu-id="bdbdb-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="bdbdb-255">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bdbdb-255">Troubleshooting</span></span>

* <span data-ttu-id="bdbdb-256">Bu genel bir özel durum işlem başlatma, büyük olasılıkla bir uygulama yapılandırma sorunu nedeniyle başarısız olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="bdbdb-257">Başvuran [dizin yapısını](xref:host-and-deploy/directory-structure), uygulamanın dağıtılan dosyaları ve klasörleri uygun ve uygulamanın yapılandırma dosyalarının mevcut olduğunu onaylayın ve ortam ve uygulama için doğru ayarları içerir.</span><span class="sxs-lookup"><span data-stu-id="bdbdb-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="bdbdb-258">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bdbdb-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
