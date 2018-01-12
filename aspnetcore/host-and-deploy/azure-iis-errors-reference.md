---
title: "Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu"
author: guardrex
description: "Sık karşılaşılan Azure uygulama hizmeti ve IIS üzerinde ASP.NET Core uygulamaları barındırdığında ayırt."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 6bd10376520a55c316a54172a70d5ae7e74db5b4
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="bb3bb-103">Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu</span><span class="sxs-lookup"><span data-stu-id="bb3bb-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="bb3bb-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bb3bb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bb3bb-105">Aşağıdaki hatalar, tam bir listesi değildir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="bb3bb-106">Burada listelenmeyen bir hata oluşursa, Lütfen ayrıntılı hata iletisi açıklamalar bölümünde bırakın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-106">Should an error occur that isn't listed here, please leave a detailed error message in the comments section below.</span></span>

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="bb3bb-107">Yükleyici VC ++ Redistributable alınamıyor</span><span class="sxs-lookup"><span data-stu-id="bb3bb-107">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="bb3bb-108">**Yükleyici özel durum:** 0x80072efd veya 0x80072f76 - belirtilmeyen hata</span><span class="sxs-lookup"><span data-stu-id="bb3bb-108">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="bb3bb-109">**Yükleyici günlük özel durum &#8224;:** hata 0x80072efd veya 0x80072f76: EXE paketini yürütülemedi.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-109">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="bb3bb-110">&#8224; Günlük C:\Users bulunduğu\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-110">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="bb3bb-111">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-111">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-112">Sistem, paket barındırma sunucusu yüklenirken Internet erişimi yoksa, bu özel durum oluştu yükleyici almasını engellendiğinde *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-112">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="bb3bb-113">Bir yükleyicisinden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-113">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="bb3bb-114">Yükleyici başarısız olursa sunucunun framework bağımlı dağıtım (FDD) barındırmak için gerekli .NET çekirdeği çalışma zamanı almayabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-114">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="bb3bb-115">Bir FDD barındırma, çalışma zamanı programlarında yüklendiğinden emin olmak &amp; özellikleri.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-115">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="bb3bb-116">Gerekirse, bir çalışma zamanı Yükleyicisi'nden elde [.NET indirmeleri](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-116">If needed, obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="bb3bb-117">Çalışma zamanı yüklendikten sonra sistemi yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-117">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="bb3bb-118">İşletim sistemi yükseltme 32-bit ASP.NET Core modül kaldırılır</span><span class="sxs-lookup"><span data-stu-id="bb3bb-118">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="bb3bb-119">**Uygulama günlüğü:** modül DLL'si **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-119">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="bb3bb-120">Veride hata yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-120">The data is the error.</span></span>

<span data-ttu-id="bb3bb-121">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-121">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-122">İşletim sistemi olmayan dosyaları **C:\Windows\SysWOW64\inetsrv** directory olmayan bir işletim sistemi sırasında korunur yükseltme.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-122">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="bb3bb-123">ASP.NET çekirdeği modülü öncesinde yüklüyse, bir işletim sistemi yükseltme ve ardından tüm AppPool çalıştırıldığında 32-bit modunda işletim sistemi yükseltme işleminden sonra bu sorunla karşılaştı.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-123">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="bb3bb-124">Bir işletim sistemi yükseltme sonrasında ASP.NET Core Modülü'nü onarın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-124">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="bb3bb-125">Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-125">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="bb3bb-126">Seçin **onarım** yükleyici çalıştırıldığında.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-126">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="bb3bb-127">RID Platform çakışıyor</span><span class="sxs-lookup"><span data-stu-id="bb3bb-127">Platform conflicts with RID</span></span>

* <span data-ttu-id="bb3bb-128">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bb3bb-128">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bb3bb-129">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\{yolu}\' komut satırı ile işlemi başlatılamadı ' "C:\\{PATH} {derleme}. { exe | dll} "', hata kodu = ' 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-129">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="bb3bb-130">**ASP.NET çekirdeği modülü günlüğü:** işlenmeyen bir özel durum: System.BadImageFormatException: '{} derlemesi .dll' dosya veya derleme yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-130">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="bb3bb-131">Yanlış biçime sahip bir program yüklemek için girişimde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-131">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="bb3bb-132">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-132">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-133">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-133">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bb3bb-134">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-134">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bb3bb-135">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-135">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bb3bb-136">Onaylayın `<PlatformTarget>` içinde *.csproj* RID ile çakışan değil.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-136">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="bb3bb-137">Örneğin, belirtmeyin bir `<PlatformTarget>` , `x86` ve bir RID yayınlama `win10-x64`, kullanarak ya da *dotnet Yayımla - c yayın - r win10-x64* veya ayarlayarak `<RuntimeIdentifiers>` içinde *.csproj*  için `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-137">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="bb3bb-138">Proje uyarı veya hata yayımlar ancak sistemdeki yukarıdaki oturum durumlarla başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-138">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="bb3bb-139">Bu özel bir Azure uygulamaları dağıtım için bir uygulama yükseltirken oluşur ve yeni derlemeleri dağıtma el ile tüm dosyaların önceki dağıtımından silerseniz.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-139">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="bb3bb-140">Uyumsuz derlemeleri kalan içinde sonuçlanabilir bir `System.BadImageFormatException` yükseltilmiş bir uygulama dağıtımı sırasında özel durum.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-140">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="bb3bb-141">URI uç nokta yanlış ya da durdurulmuş Web sitesi</span><span class="sxs-lookup"><span data-stu-id="bb3bb-141">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="bb3bb-142">**Tarayıcı:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="bb3bb-142">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="bb3bb-143">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bb3bb-143">**Application Log:** No entry</span></span>

* <span data-ttu-id="bb3bb-144">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bb3bb-144">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bb3bb-145">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-145">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-146">Doğru URI uç nokta uygulaması için kullanılan onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-146">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="bb3bb-147">Bağlamaları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-147">Check the bindings.</span></span>

* <span data-ttu-id="bb3bb-148">IIS Web sitesinin içinde olmadığını onaylayın *durduruldu* durumu.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-148">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="bb3bb-149">Devre dışı CoreWebEngine veya W3SVC sunucusu özellikleri</span><span class="sxs-lookup"><span data-stu-id="bb3bb-149">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="bb3bb-150">**İşletim sistemi özel durum:** ASP.NET Core modülü kullanmak için IIS 7.0 CoreWebEngine ve W3SVC özellikleri yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-150">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="bb3bb-151">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-151">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-152">Uygun rol ve özellikleri etkinleştirildiğini onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-152">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="bb3bb-153">Bkz: [IIS yapılandırmasını](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-153">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="bb3bb-154">Yanlış Web sitesi fiziksel yolu veya uygulama eksik</span><span class="sxs-lookup"><span data-stu-id="bb3bb-154">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="bb3bb-155">**Tarayıcı:** 403 Yasak - erişim reddedildi **--veya--** 403.14 Yasak - Web sunucusu bu dizinin içindekileri listelemeyecek şekilde yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-155">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="bb3bb-156">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bb3bb-156">**Application Log:** No entry</span></span>

* <span data-ttu-id="bb3bb-157">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bb3bb-157">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bb3bb-158">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-158">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-159">IIS Web sitesini denetleyin **temel ayarları** ve fiziksel uygulama klasör.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-159">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="bb3bb-160">Uygulama IIS Web sitesinde klasöründe olduğunu onaylayın **fiziksel yolu**.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-160">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="bb3bb-161">Yanlış rol, modül yüklü değil veya yanlış izinler</span><span class="sxs-lookup"><span data-stu-id="bb3bb-161">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="bb3bb-162">**Tarayıcı:** iç sunucu hatası 500.19 - sayfayla ilgili yapılandırma verileri geçersiz olduğundan istenen sayfaya erişilemiyor.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-162">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="bb3bb-163">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bb3bb-163">**Application Log:** No entry</span></span>

* <span data-ttu-id="bb3bb-164">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bb3bb-164">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bb3bb-165">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-165">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-166">Uygun rolde etkinleştirilmiş olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-166">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="bb3bb-167">Bkz: [IIS yapılandırmasını](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-167">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="bb3bb-168">Denetleme **programlar &amp; özellikleri** onaylayın **Microsoft ASP.NET Core Modülü** yüklendi.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-168">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="bb3bb-169">Varsa **Microsoft ASP.NET Core Modülü** yüklü programlar listesinde yer almayan modülünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-169">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="bb3bb-170">Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-170">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="bb3bb-171">Olduğundan emin olun **uygulama havuzu** > **işlem modeli** > **kimlik** ayarlanır **ApplicationPoolIdentity** veya özel kimlik uygulamanın dağıtım klasörüne erişmek için doğru izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-171">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="bb3bb-172">Yanlış processPath, eksik PATH değişkeni, barındırma paketi yüklü değil, sistem/IIS yeniden, VC ++ yüklü değil, Redistributable veya dotnet.exe erişim ihlali</span><span class="sxs-lookup"><span data-stu-id="bb3bb-172">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="bb3bb-173">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bb3bb-173">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bb3bb-174">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' ".\{ derleme} .exe"', hata kodu = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-174">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="bb3bb-175">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş</span><span class="sxs-lookup"><span data-stu-id="bb3bb-175">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="bb3bb-176">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-176">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-177">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-177">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bb3bb-178">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-178">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bb3bb-179">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-179">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bb3bb-180">Denetleme *processPath* özniteliği `<aspNetCore>` öğesinde *web.config* olduğundan emin olmak için *dotnet* framework bağımlı dağıtım (FDD) veya *. \{derleme} .exe* müstakil dağıtımı (SCD).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-180">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="bb3bb-181">Bir FDD için *dotnet.exe* yol ayarları erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-181">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="bb3bb-182">Onaylayın * C:\Program Files\dotnet\* sistem yolu ayarlarında bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-182">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="bb3bb-183">Bir FDD için *dotnet.exe* uygulama havuzu kullanıcı kimliği için erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-183">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="bb3bb-184">Uygulama havuzu kullanıcı kimliği için erişimi olduğunu doğrulamak *C:\Program Files\dotnet* dizini.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-184">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="bb3bb-185">Uygulama havuzu kullanıcı kimliği için yapılandırılmış hiçbir izin verme kuralı olduğundan emin olun *C:\Program Files\dotnet* ve uygulama dizinleri.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-185">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="bb3bb-186">Bir FDD dağıtılan ve IIS'yi yeniden başlatmadan .NET Core yüklü.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-186">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="bb3bb-187">Sunucuyu yeniden başlatın veya yürüterek IIS'yi yeniden **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-187">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bb3bb-188">Bir FDD barındıran sistemde .NET çekirdeği çalışma zamanı yüklemeden dağıtılmış.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-188">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="bb3bb-189">.NET çekirdeği çalışma zamanı yüklenmemiştir çalıştırırsanız **.NET Core Windows Server barındırma Paket Yükleyici** sistemdeki.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-189">If the .NET Core runtime hasn't been installed, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="bb3bb-190">Bkz: [.NET Core Windows Server barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-190">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="bb3bb-191">Internet bağlantısı olmadan bir sistemde .NET çekirdeği çalışma zamanı yükleme girişimi varsa, çalışma alanından elde [.NET indirmeleri](https://www.microsoft.com/net/download/core) ve ASP.NET Core modülünü yüklemek için barındırma paket yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-191">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="bb3bb-192">Sistemi yeniden başlatmayı veya yürüterek IIS yeniden yüklemeyi tamamlamak **net stop edildi /y** arkasından **net start w3svc** bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-192">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bb3bb-193">Bir FDD dağıtılan ve *Microsoft Visual C++ 2015 Redistributable (x64)* sistemde yüklü değil.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-193">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="bb3bb-194">Bir yükleyicisinden elde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-194">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="bb3bb-195">Yanlış bağımsız \<aspNetCore\> öğesi</span><span class="sxs-lookup"><span data-stu-id="bb3bb-195">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="bb3bb-196">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bb3bb-196">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bb3bb-197">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' "dotnet".\{ .dll derleme}', hata kodu = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-197">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="bb3bb-198">**ASP.NET çekirdeği modülü günlüğü:** yürütmek için uygulama yok: ' yolu\{derleme} .dll '</span><span class="sxs-lookup"><span data-stu-id="bb3bb-198">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="bb3bb-199">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-199">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-200">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-200">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bb3bb-201">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-201">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bb3bb-202">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-202">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bb3bb-203">İncelemek *bağımsız değişkenleri* özniteliği `<aspNetCore>` öğesinde *web.config* ya da olduğunu onaylamak için (a) *.\{ derleme} .dll* framework bağımlı dağıtım (FDD); veya (b yok, boş bir dize) (*bağımsız değişkenleri = ""*), veya bir uygulamanın bağımsız değişkenleri listesi (*bağımsız değişkenler "arg1, arg2,..." =*) kendi içinde bulunan dağıtımlar için (SCD).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-203">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it is either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="bb3bb-204">Eksik .NET Framework sürümü</span><span class="sxs-lookup"><span data-stu-id="bb3bb-204">Missing .NET Framework version</span></span>

* <span data-ttu-id="bb3bb-205">**Tarayıcı:** 502.3 hatalı ağ geçidi - bağlantı hatası isteği yönlendirmek çalışılırken oluştu.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-205">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="bb3bb-206">**Uygulama günlüğü:** HataKodu = uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' komut satırı ile işlemi başlatılamadı ' "dotnet".\{ .dll derleme}', hata kodu = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-206">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="bb3bb-207">**ASP.NET çekirdeği modülü günlüğü:** yöntemi, dosya veya derleme özel durum eksik.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-207">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="bb3bb-208">Yöntemi, dosya veya derleme özel durum belirtildi bir .NET Framework yöntemi, dosya veya derleme olabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-208">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="bb3bb-209">Sorun giderme:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-209">Troubleshooting:</span></span>

* <span data-ttu-id="bb3bb-210">Sistemden eksik .NET Framework sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-210">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="bb3bb-211">Bir framework bağımlı dağıtım (FDD) için doğru çalışma zamanı sistemde yüklü olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-211">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="bb3bb-212">Proje 1.1 barındırma sisteme dağıtılan 2.0 sürümüne yükseltilir ve bu özel durumun sonucu, 2.0 framework barındıran sistemde olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-212">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="bb3bb-213">Durdurulan uygulama havuzunu</span><span class="sxs-lookup"><span data-stu-id="bb3bb-213">Stopped Application Pool</span></span>

* <span data-ttu-id="bb3bb-214">**Tarayıcı:** 503 Hizmet kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="bb3bb-214">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="bb3bb-215">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bb3bb-215">**Application Log:** No entry</span></span>

* <span data-ttu-id="bb3bb-216">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulmadı</span><span class="sxs-lookup"><span data-stu-id="bb3bb-216">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="bb3bb-217">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bb3bb-217">Troubleshooting</span></span>

* <span data-ttu-id="bb3bb-218">Uygulama havuzu içinde olmadığını onaylayın *durduruldu* durumu.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-218">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="bb3bb-219">IIS tümleştirme Ara uygulanmadı</span><span class="sxs-lookup"><span data-stu-id="bb3bb-219">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="bb3bb-220">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bb3bb-220">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bb3bb-221">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH}\{derleme}. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =</span><span class="sxs-lookup"><span data-stu-id="bb3bb-221">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="bb3bb-222">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve normal işlemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-222">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="bb3bb-223">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bb3bb-223">Troubleshooting</span></span>

* <span data-ttu-id="bb3bb-224">Uygulamayı yerel olarak Kestrel üzerinde çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-224">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bb3bb-225">İşlem hatası uygulamada bir sorun sonucu olabilir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-225">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bb3bb-226">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-226">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="bb3bb-227">Ya da onaylayın:</span><span class="sxs-lookup"><span data-stu-id="bb3bb-227">Confirm that either:</span></span>
  * <span data-ttu-id="bb3bb-228">Referencedby IIS tümleştirme Ara yazılımıdır çağırma `UseIISIntegration` uygulamanın yöntemi `WebHostBuilder` (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="bb3bb-228">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="bb3bb-229">Uygulamaları kullanan `CreateDefaultBuilder` yöntemi (ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-229">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="bb3bb-230">Bkz: [ASP.NET Core barındırma](xref:fundamentals/hosting) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-230">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="bb3bb-231">Alt uygulama içeren bir \<işleyicileri\> bölümü</span><span class="sxs-lookup"><span data-stu-id="bb3bb-231">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="bb3bb-232">**Tarayıcı:** HTTP Hatası 500.19 - iç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="bb3bb-232">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="bb3bb-233">**Uygulama günlüğü:** girişi yok</span><span class="sxs-lookup"><span data-stu-id="bb3bb-233">**Application Log:** No entry</span></span>

* <span data-ttu-id="bb3bb-234">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturulur ve kök uygulaması için normal işlem gösterir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-234">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="bb3bb-235">Günlük dosyası için alt uygulaması oluşturulmamış.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-235">Log file not created for the sub-app.</span></span>

<span data-ttu-id="bb3bb-236">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bb3bb-236">Troubleshooting</span></span>

* <span data-ttu-id="bb3bb-237">Onaylayın alt uygulamanın *web.config* dosya içermez bir `<handlers>` bölümü.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-237">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="bb3bb-238">Uygulama yapılandırma genel sorunu</span><span class="sxs-lookup"><span data-stu-id="bb3bb-238">Application configuration general issue</span></span>

* <span data-ttu-id="bb3bb-239">**Tarayıcı:** HTTP hatası 502.5 - işlem hatası</span><span class="sxs-lookup"><span data-stu-id="bb3bb-239">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bb3bb-240">**Uygulama günlüğü:** uygulama ' MACHINE/WEBROOT/APPHOST / {DERLEMESİ} ' fiziksel kök ile ' C:\\{PATH}\' işlem komut satırı ile oluşturulan ' "C:\\{PATH}\{derleme}. { exe | dll} "' ancak kilitlendi veya değil yanıt vermedi ya da verilen bağlantı noktası üzerinde '{PORT}', ErrorCode dinleme değil '0x800705b4' =</span><span class="sxs-lookup"><span data-stu-id="bb3bb-240">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="bb3bb-241">**ASP.NET çekirdeği modülü günlüğü:** günlük dosyası oluşturuldu ancak boş</span><span class="sxs-lookup"><span data-stu-id="bb3bb-241">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="bb3bb-242">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="bb3bb-242">Troubleshooting</span></span>

* <span data-ttu-id="bb3bb-243">Bu genel bir özel durum işlem başlatma, büyük olasılıkla bir uygulama yapılandırma sorunu nedeniyle başarısız olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-243">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="bb3bb-244">Başvuran [dizin yapısını](xref:host-and-deploy/directory-structure), uygulamanın dağıtılan dosyaları ve klasörleri uygun ve uygulamanın yapılandırma dosyalarının mevcut olduğunu onaylayın ve ortam ve uygulama için doğru ayarları içerir.</span><span class="sxs-lookup"><span data-stu-id="bb3bb-244">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="bb3bb-245">Daha fazla bilgi için bkz: [sorun giderme](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bb3bb-245">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
